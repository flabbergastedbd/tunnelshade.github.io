---
layout: post
title: "How I created dev environment for OWTF"
categories: linux
tags: linux owtf
image: https://www.owasp.org/images/0/02/OWTFLogoSmall.png
---

Go to the [contributor's wiki](https://github.com/7a/owtf/wiki/Contributor%27s-README). All the rules are present there :P
The following is a self made reminder post XD  
Let me go through this post in a **Q&A** format so that you can craft up your own solutions if you like. 

</br>
###OWTF is supposed to be run on Kali


**Problem** :- But kali is not regularly used as a main distro. Kali
is generally used inside a VM. So install an editor inside kali bla bla
bla...

**My Solution** :- Use shared folders in your virtualization software. I use **VirtualBox**,
 in which guest additions are supposed to be installed to use shared
folders. Enable bi-directional clipboard as well. So you can now edit
your code in your **HOST OS**.


**Problem** :- Still painful to switch between guest and host for editing and running code???

**My Solution** :- Start the ssh server in kali, and connect to it. As owtf is a command line utility you shouln't have a problem.


*NOTE:* Donot forget to run owtf inside your shared directories so that you can view the report in your Host OS itself (Iceweasel sux).

</br>
###Debugging in OWTF


**Problem** :- OWTF has **two important logs**, how to open them simultaneously each time you run OWTF?

**My Solution** :- Use an intelligent terminal like **urxvt** which lets you click on file links and execute a particual command(i.e open those files in a new tab)



**Problem** :- How the hell do you expect me to **switch** between these logs??

**My Solution** :- Use a **tiling window manager** (or) atleast use a terminal emulator which can be split (**terminator)**:P

![OWTF Running](http://i43.tinypic.com/2mfi6q8.jpg)

**Problem** :- OWTF is not exiting cleanly i.e lots of zombie processes XD

**My Solution** :- Keep this in your .bashrc

{% highlight bash %}
kill -9 `ps | grep -v "bash$\|ps$\|grep$\|CMD$\|awk$" | awk '{ print $1 }'
{% endhighlight %}
