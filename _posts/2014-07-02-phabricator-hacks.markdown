---
layout: post
title: "Phacilitating phew bugs ;)"
categories: Hacks
tags: tricks hacks
image: http://phacility.com/rsrc/image/phacility/phacility_logo.jpg
---

I am not a huge fan of bug bounties since I am more of a tool developer. But as the title suggests, to keep myself
fresh & bounties from [**IBB**](https://hackerone.com/ibb) are special. So enter **Phabricator**.

##### Phabricator is :

+ the best piece of software for collaboration
+ originally written at _Facebook_
+ now maintained by [_Phacility_](http://phacility.com)
+ open [source](https://github.com/phacility)

##### Bugs (Great that all the details are on hackerone, no need to repeat ;)

+ https://hackerone.com/reports/16315
+ https://hackerone.com/reports/16392
+ https://hackerone.com/reports/18691 (*Not yet public at the time of writing)

##### What to grasp :

+ Focus on functional bugs, there will be lots of design flaws.
+ When testing something like phabricator which requires your own installation, use a **VPS**. Setup all the required
stuff and then save a snapshot. This way even if you mess up your installation, just restore using snapshot.
+ Be persistent and calm.
+ Find bugs :P.

Enough of **phabricator** for now. Lets build some stuff ;)