---
title: "Giving Back"
date: 2015-09-13T11:43:23Z
permalink: "/posts/giving-back/"

---

I’ve been a fan of open source software since I found out about Linux. I remember learning about how you could install software for free and how people could help make that software better for free too, and how people did this purely just to help others. This started my fascination with installing new or different versions of Linux, and with each install I'd gain more insight into the open source community.

While living my virtual life inside of these community-made metropolises, I always had the feeling that I was stealing my time within it, like a kid sneaking under turnstiles at an amusement park. While the developers of it obviously knew people would make use of it without paying, they were giving something away to make the wider picture a prettier one. And here I was, Instagram-ing it and not giving attribution, quoting it without referencing it, using it and not giving anything back.

## Time for a change ##

Now that I’m developing software and helping build products for a living, there really isn’t a better time for me to get involved in open source software development. I even think it will teach me a lot in addition to my work - more feedback from developers of different backgrounds, seeing projects take off and then pivot in other directions (or even failing), using a variety of technologies I might not use in my day-to-day.

That’s when I decided to make something.

## Make is the operative word ##

Right now, most of my time spent on a laptop is spent developing software; at work, at home on little side projects. So this seemed like a good place to make something to make this easier. I looked at what I was good at; right now, it’s probably Python. But I couldn’t see anything that I could really improve on because there is such a wealth of brilliant libraries out there ([Requests](https://github.com/kennethreitz/requests), I’m looking at you). And my workflow for Python development was pretty good, thanks to PyCharm.

But web development on the other hand, is something I want to be better at. Not just in terms of [career prospects](http://mashable.com/2015/01/18/programming-languages-2015/#stlTE_c.aEkB), but also in terms of interest. Building something to make web development easier, perhaps using web development technology, could be a win-win - I’d learn more while making it, and it’s creation would help me further!

What frustrated me most about web development? Layout. It will look good one minute, and then you realise it’s not responsive, so you try and fix that and then you realise the widescreen version now looks off. And you have hidden sizing issues that only appear in certain browsers and certain resolution and oh it’s all a bit of a mess.

What if I could easily see elements on my rendered page, and at a glance know the area they own on a page?

## The idea - BuildingBlocks ##

Like a wall of Lego, I wanted to see each block and more importantly, how they connect with the blocks around it.

I started by reading a little more about [developing a Javascript module](http://blog.teamtreehouse.com/how-to-write-a-module-in-javascript-quick-tip). Javascript made sense as it’s the language of the web and it’s popularity is going through the roof. While I’ve mingled with it (specifically, [Backbone.js](http://backbonejs.org/) for a university project and some simple element interaction stuff at work), I’ve never seriously written it, especially not in a way that would make it useful to others. So this was a bit new for me and I was feeling that tingly learning feeling again.

## The implementation ##

I got to coding; writing in [Brackets](http://brackets.io/), with a test page and live preview making the development process a smooth one. The premise is really simple; you give the module an element, and it will choose a random colour and assign that to the elements background-color style attribute. This way, you could use the module while developing and see exactly the size of the elements on the page, thus making the layout easier to develop.

I made some pretty hefty and questionable decisions early on; I decided that it should instead take a PARENT node and it would colour in the child nodes, but only the child nodes that are of a certain tag type (like h1, or p). It made sense at the time, but I wish I’d written down the reasoning because now I’m quite confused about it (by that, I mean "WTF was I thinking?")

After a couple of hours of hacking away and some meandering through [colour theory](http://www.colormatters.com/color-and-design/basic-color-theory), I had something. It wasn't much, but it worked. For me, this was like the first marriage counseling session for me and web development.

## The future of BuildingBlocks ##

In it's current state, it's a little... rough. It's not interactive with page resizing (what), it's got to be statically loaded onto the page in question (WHAT), and that little gem I mentioned earlier about parent nodes and child nodes of a certain tag type (WAHT).

This makes it pretty useless for savvy web devs and actually, everyone else. The next step will be a browser extension. This will allow BuildingBlocks to do its thing on ANY page you want, without being statically loaded. I also want to make it more configurable regarding where it starts colouring elements. The ideal scenario for me would be to allow for clicking an element, and it would colour from there down. It should also maybe trigger on hover events.

## The future of my Open Source adventure ##

![the start of something?](/assets/images/giving-back/buildingblocks_contributions.png)

This was my first, small venture into open source. It might seem insignificant in the grand scheme of things (because it kind of is), but I think it's been successful in what it set out to do - to get me writing open source software. I'm hoping this continues, as I figure out what should be written to help others, either as a software consumer or a software developer. For now, I'm going to try and follow a mantra:

> ~~be~~ build the ~~change~~ project you want to see in the world



