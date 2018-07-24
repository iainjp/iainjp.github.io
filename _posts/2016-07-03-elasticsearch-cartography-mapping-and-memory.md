---
title: "Elasticsearch Cartography Mapping and Memory"
date: 2016-07-03T12:27:25Z
tags:
 - "elasticsearch"
---


*I wrote this a while ago but took ages to get it online. When I mention Elasticsearch, it's specifically Elasticsearch 1.x. We'll likely upgrade to Elasticsearch 2.x in the near future!*


## How we use Elasticsearch

My team at work use a combination of Kafka, Elasticsearch and Samza for reporting on email communications. My colleague Mira has [written a brilliant post about how this all hangs together](http://blog.mirajavora.com/sendgrid-webhooks-reporting/). All in all, it’s a great solution that provides details of how effective we are at communicating with our users. In fact, its such a great solution that our friends all over the business are using it. In particular, those in charge of email content for different countries are using it to check their content is a good market fit for their demographic.


### When the problems started...
This increase in use was starting to cause us some issues, however. We were noticing, and receiving reports of users noticing some pretty scary error messages in Kibana. These messages would often accompany our dashboards failing to load data. In some cases, the dashboards seemingly would load but displayed incomplete and incorrect data. 

![Uh oh, a thing is totally broken](/assets/images/elasticsearch-cartography-mapping-and-memory/ES_memory_error.png)


These errors were happening because Elasticsearch was running out of memory to store shards while performing aggregations on them. To understand why, we have to look at how Elasticsearch stores data in memory for these aggregations, and what Elasticsearch does for new fields without an index mapping.


## How Elasticsearch uses memory

An Elasticsearch index is keyed by terms, which give you a list of the documents containing those terms. This makes sense, as it lets you efficiently answer questions like *"What documents match these terms?"*.

![An Elasticsearch index](/assets/images/elasticsearch-cartography-mapping-and-memory/ES_index.jpg)

However, aggregations over this would be difficult. Imagine asking the question "how many documents contain the term 'discovery'?". We would need to traverse all the documents to answer this question, which just doesn't scale. So instead, Elasticsearch creates **Fielddata**.

![Now we can answer those aggregation questions easily!](/assets/images/elasticsearch-cartography-mapping-and-memory/ES_fielddata.jpg)

How these terms are stored depends on the **index mapping**, which tells Elasticsearch "This field is a string containing different terms that I want to query across, but this field should be treated this a single values that I want to match only if the input string is an exact match".


### What Elasticsearch does for new fields without an index mapping

Elasticsearch can be used without defining a schema upfront. Behind-the-scenes, it uses dynamic mapping to infer the field type and the properties of that field. It then adds this information to the index mapping to make it a known field. 

For string fields, dynamic mapping results is an **analysed and tokenised** field. This results in the value **emailtype\_a\_variant** being split into the terms **emailtype**, **a**, **variant**, which could be matched regarless of case. While this is a great default for search engine-like projects with information retrieval priorities like relevance, it’s not what we want - we only really need exact matches. This tokenised and analysed string also takes up more room in Field data.
 

#### Our mapping had gone off course...
 At the start of the project, we were ingesting a lot less data into Elasticsearch. However, as our use cases grew, so did the requirements for more fields. We added this data, and let Elasticsearch take care of the mappings for us. However it was now becoming apparent that this wasn’t the ideal scenario due to memory use. 




### Solution attempt #1 - Make Elasticsearch clear it's memory

A surprising property of Fielddata is that by default, Elasticsearch never evicts this from memory. With a single config value, we could tell ES to evict items from memory once memory use got about a certain threshold. We did this, setting the eviction threshold to a number of values between 50% and 70%. 

![Elasticsearch memory usage over time](/assets/images/elasticsearch-cartography-mapping-and-memory/ES_memory_usage_over_time-orig.jpg)

The process of eviction is really aggressive for this and caused Elasticsearch to block requests and become unresponsive for users. We also noticed that ES would often get into a pickle, whereby it would be just below the threshold while at rest, and a simple query would push it above the threshold causing eviction, which would bring it down just below the threshold again and so on. This wasn’t what we were wanting at all, so we reverted the change and looked for other solutions.



### Solution #2 - Reduce Elasticsearch heap footprint in the first place

This was a lengthier attempt at solving the issue. We took a good hard look at the index mapping that Elasticsearch had generated for our new fields and decided which of these could be changed to no longer be analysed and tokenised. **It was pretty much all of them!** This should reduce memory footprint as we’ll no longer be storing the field as multiple separate terms.

![Memory use after the mapping change](/assets/images/elasticsearch-cartography-mapping-and-memory/ES_better_memory_profile.jpg)

A brilliant side-effect of this change is that we can make use of doc values. Doc values are an alternative to the in-memory inverted index that fielddata provides, which is instead created when a document is indexed and stored on disk as opposed to in-memory. Elasticsearch (as of version 1.6) doesn’t support using doc values for analysed strings, but this is [set to change](https://github.com/elastic/elasticsearch/issues/12394). 


### End results

After investing the time to re-index our data into the new format, we noticed very little heap use. So low in fact, that we haven't received a single Out of Memory issue complaint again! This is great - our friends around the business now trust our service more, we now trust the service more, and the data itself is in the best format for what we needed to do. 

### Future Work

There are still further improvements we want to make. For example, our data set is now increasing at a much faster rate - this means that we can no longer do aggregations of large periods of time as there is simply too much data for our cluster to work through. Instead, we might look to separate our concerns (good old engineering principles to the rescue yet again) and publish our data into another system more capable of larger aggregations and use that for trend insights instead. 

I'll make sure to post about that if and when it happens.


