---
title: "Adventures in Naming Things"
date: 2016-01-24T12:20:41Z
draft: false
tags:
 - "naming is hard"
 - "clean code"
 - "naming things"
 - "design patterns"
 - "uncle bob"
---

Naming things is hard. This isn't a revelation (see [Martin Fowler, via Phil Karlton](http://martinfowler.com/bliki/TwoHardThings.html)), and many more people will say it after me. A name is how we communicate intent to the next reader of our code. In order to communicate that intent, the name needs to be purposeful.

![Phil Karlton quote. Image: http://www.slideshare.net/gwadeloop/high-profile-editorialsitesv5](/assets/images/adventures-in-naming-things/phil_karlton_quote.jpg)


I've been thinking about good naming and have recently experimented with different naming conventions to what I'm used to.


### Naming after patterns


In the past, I've often based names after the patterns in use. If something is mapping from one domain object to another, like from a `UserDto` to a `SubscriberDto`, it could be called a `SubscriberMapper`, with a method signature like `SubscriberDto Map(UserDto user)`. Maybe this is from the projects I've worked in - many have been .NET projects or other languages in a team with experienced .NET developers - but to this day it's often how I name things. Is this really enterprise-y? I don't know. Not even sure if I care.


### Domain-specific naming

Recently while playing around with Javascript with [BuildingBlocks](https://iainjp.com/giving-back/), I scrapped the naming conventions I've picked up from working in .NET (with `mapper`, `builder`, `factory`, `wrapper` etc. all being a part of the name). Instead I gave them domain-specific names. The component that colours in page elements is called `Painter`. There's something that generates ids called `Generator`. Another that creates names for elements based on their position called a `Describer`.

----


It was pretty interesting changing this. It made thinking about the structure, especially segregating responsibility a bit easier. Perhaps this is because the entities are named like physical objects. A painter will paint, a describer will describe. Where do I get the id for this element? I need to generate it, so I'll ask the generator.


### Criticism of this

Looking this up more though, it's not popular with some more experienced engineers of object-oriented software.

[Objology](http://objology.blogspot.co.uk/2011/09/one-of-best-bits-of-programming-advice.html) writes that not naming things ending in "er" is the best advice they ever received, describing that in Object Oriented Programming, we should think about nouns and data, and *then* attach behaviour and *"hope that the problems we hope to solve gel out of the emergent behaviors"*. I don't really agree with this - we should use objects to encapsulate cohesive data and behaviour, and I'd rather start with the behaviour I want to introduce.

Uncle Bob mentions noise words in "Clean Code" (chapter 2). Maybe my verb-y names are similar to these? Uncle Bob was mainly calling out unnecessary affixes, like Data and Info, so perhaps not.

![Uncle Bob, Clean Code. 10/10, would recommend](/assets/images/adventures-in-naming-things/clean_code.jpg)

---

While I probably won't use this much at work, especially in our .NET projects that have fairly standardised naming conventions, it was quite interesting to change the naming in my personal project. It meant I did some research about naming, but also noticed the effects that different naming can have - it actually changed the way I mentally modelled the problem, and probably how I implemented it too. 
