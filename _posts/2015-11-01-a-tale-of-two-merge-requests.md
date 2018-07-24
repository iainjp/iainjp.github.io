---
title: "A Tale of Two Merge Requests"
date: 2015-11-01T11:50:01Z
draft: false
tags:
 - "git"
 - "getting things done"
 - "process"
permalink: "posts/a-tale-of-two-merge-requests"
---

With the new intake of graduates settling into their roles at my job, all fresh-faced and eager to learn the tricks of their trade, we have to remember that they are not the only ones with things still left to learn. As a graduate from last years intake (now relegated to "Old Grad") this was all too relevant for me last week, when I learned a lesson in Getting Things Done, or in this case, Getting Smaller Things Done And Not Making Too Many Sweeping Changes to Code, Especially If Others Are Working On That Code At The Same Time (not as catchy). 


##### What we were doing #####

I was working with a teammate on a service we own, implementing a new automated email type. The work was cut in two. One card was based around the Scheduler, which handles - you guessed it - the schedules for the communications to be built. The other card was for work on the Loader, which gathers all the information required to build up the communication, and fires this over to our email publishing service. This makes complete sense - these are two separate components, each with specific jobs in the context of this new email type. 

With two cards, each tackling a different "side" of the feature, we got to work. 

##### What was meant to happen #####

The tasks were meant to be tackled in parallel. Each wouldn't take long, as very similar work was done prior on another email type. Following the example set by earlier work, implementation would take about a day, and would be merged the following morning. The sun would be shining and all would be well in the world. 

![Cue animated animal friends and a flash dance.](/assets/images/a-tale-of-two-merge-requests/cue_animated_birds.jpg)

##### What really happened... #####

The tasks were tackled in parallel. The work done for the earlier email type was almost identical to the work needed now. We weren't going to duplicate this knowledge in the service, especially since we have exciting new comms on the horizon that would also require similar implementation. Instead, we took [Kent Becks](https://twitter.com/kentbeck/status/250733358307500032) advice - "Make the change easy, then make the easy change" - and refactored some of the work done for the earlier comm in order to reduce duplication and encourage re-use. 


<blockquote class="twitter-tweet center" lang="en"><p lang="en" dir="ltr">for each desired change, make the change easy (warning: this may be hard), then make the easy change</p>&mdash; Kent Beck (@KentBeck) <a href="https://twitter.com/KentBeck/status/250733358307500032">September 25, 2012</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>




Except, this attempt at re-use happened as part of both cards simultaneously, with little communication of the changes being made. With two attempts at **altruism**, we eventually created **chaos**. The work itself took about a week, creating two very large merge requests (one of which was too large for Gitlab to display). The merge requests themselves spanned more than 2 days. That's two full days of comments (which were quite strict, as we knew a lot of the implementation would be used again in the near future), amendments, and twice telling the squad at stand-up "I'll be working on the merge request again today...". On Friday, both cards were moved into *Ready to Deploy* and waiting in the Master branch to be released. 

![97 messages, 77 changes, lots and lots of time](/assets/images/a-tale-of-two-merge-requests/mr_changes_with_highlights.png)

Work: **done**, CI pipeline: **green**, and the code base is in a better state, thanks to the refactoring. I should feel happy with my accomplishments. But I didn't. That Friday, I looked back on the week and felt like I didn't achieve anywhere near as much as was possible, and yet I definitely did work hard because I was *exhausted*. 

![Gitlab was not impressed...](/assets/images/a-tale-of-two-merge-requests/gitlab_was_not_chuffed.png)


##### Why didn't this happen as planned? #####

While the work done was good, a number of things could have been done to make this more efficient: 

 * **Planning**: Instead of rushing in to start on the two independent cards, we could have paired on the intersection of this work - the refactor of the similarities with the previous work. From here, we would have had a good, re-usable base to work on the two separate components in isolation. And pairing would have meant we would be more confident with this implementation, reducing the need to be extra strict with merge request comments. 

  * **Communication**: There could have been better communication when implementing the changes. We're starting to use Slack more heavily in the squad  - this would have been a great time to put aside the gifs and give each other a heads-up about big changes or ask for a second opinion on some design decisions. 

* **Merging early and often**: It wasn't necessary to have waited until everything was complete on both cards before we started to merge into Master. Chunking the work into smaller parts for more easy-to-comprehend merge requests would have worked really well here. The use of scheduling equates to something similar to a feature flag - the new code wouldn't execute without a schedule, and the loader would never receive the new type of comm without the scheduler adding one to the queue - so we could have merged in smaller parts. 

##### Actions I'm taking forward #####

While I didn't feel like I got enough done, perhaps I should be happy with what I've learned. Going forward, I'm going to focus on better planning for the work and spotting opportunities to pair early on in tasks. There's going to be more communication about tasks as they're being done. There is also going to be more than one merge request per feature (the new [WIP feature in Gitlab](http://doc.gitlab.com/ce/workflow/wip_merge_requests.html) should make this really simple to do!). 

Maybe this has happened to you recently, or you've seen it happen elsewhere. If this is the case, I'd advise **planning**, **communication** and **merging early and often**. 


I'm always open to learning more about process, so give me a shout or comment here with anything you'd like to add! 

