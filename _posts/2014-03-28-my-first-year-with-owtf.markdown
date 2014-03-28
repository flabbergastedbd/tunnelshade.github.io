---
layout: post
title: "One year with OWTF!"
categories: information
tags: info
image: https://www.owasp.org/images/0/02/OWTFLogoSmall.png
---

Well, weird things happen in many ways ;). I always wanted to build a security tool which will be used by people (simple dreams that every skiddie has).
Last year around this time when I was accidentally going through the list of accepted organizations for GSoC 2013, I visited 
[**OWASP**'s Ideas page](https://www.owasp.org/index.php/GSoC2013_Ideas#OWASP_OWTF_-_Inbound_Proxy_with_MiTM_and_caching_capabilities)
and ended up sending mails to two projects. What followed was pure black magic :P. I selected [**OWTF**](https://github.com/owtf) project to continue with the GSoC process &
somehow my proposal got accepted. Well, this is how it started, but to be precise, even before I was accepted I started contributing because the project 
was really interesting ;)

My first contribution was [a simple bug fix](https://github.com/owtf/owtf/pull/15/), which was difficult at that time considering the huge size of OWTF.


#### List of my main contributions
+ Helped in [porting](https://github.com/owtf/owtf/commit/f6b0d64afc740a22f6ca0a5f8e4aca66ddd71986) OWTF to Kali Linux.
+ Made owtf stable on Kali because of which ["v0.16 shady citizen"](http://blog.7-a.org/2013/05/owasp-owtf-016-shady-citizen-released.html) was released.
+ Built [fastest MiTM python proxy](http://blog.7-a.org/2013/08/owtf-030-summer-storm-ii-released-plz-rt.html) for OWTF as a part of GSoC 2013.
+ Implemented a different [installation procedure](https://github.com/owtf/owtf/commit/785dacfc5a96a83cf6e8944ffafb79862699498c) in owtf for supporting multiple distributions.
+ Implemented an [update mechanism](https://github.com/owtf/owtf/commit/a5cfd4f5e3b51ffbcdbd19e9eca35a1c346f2976) for stable and bleeding edge updates.
+ Helped in a total of [4 releases](https://github.com/owtf/owtf/releases).


#### Some Git Stats
+ **Number of commits** : 163 (
```
git shortlog -sn
```
)
+ **Bugs fixed + Features added** : 75 (
```
git log --pretty=oneline --author="tunnelshade@gmail.com" | grep -i "fix" | wc -l
```
)
+ **Lines added** : 16550 ([Check these commands](http://codeimpossible.com/2011/12/16/Stupid-Git-Trick-getting-contributor-stats/))
+ **Lines deleted** : 13886


These stats are just for fun & I can confirm that my contributions to this awesome project are nothing compared to what I got from OWTF.
I am unable to come up with a word to describe my fantastic experience with OWTF so far. OWTF along with this blogpost will remain incomplete 
without mentioning **Abraham Aranguren**, the founder & project leader for **O**ffensive **W**eb **T**esting **F**ramework. I really don't think 
there can be a better project leader **:D**. I am glad to have him as my project leader.

#### Happenings with OWTF!
+ I will be mentoring a OWTF project during **GSoC 2014**.
+ Some serious development is going on in [**UI** branch](https://github.com/owtf/owtf/tree/ui) of our repo ;)
+ Check out the [ideas of OWTF](https://www.owasp.org/index.php/GSoC2014_Ideas) for GSoC 2014 to know about future features (There are some hidden surprises as well)
+ Next version including [**Botnet Mode**](http://blog.7-a.org/2013/12/owasp-owtf-cfp-funds-contest-winners.html) will be out before GSoC 2014 *coding* period.
