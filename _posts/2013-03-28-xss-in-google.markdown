---
layout: post
title: "XSS in Google 500 Error page"
categories: Hacks
tags: hacks info
image: https://www.google.com/images/srpr/logo11w.png
---

Due to highly poor internet conditions in my university, we often encounter google
error pages. But this happened during my winter break when googling some stuff,
I was redirected to a Google 500 error page. What caught my attention was that
arguments were present there but the page was a html page. So I thought of viewing
the source and I saw that, just some regex matching using javascript was done to
extract those parameters. The parameters were being passed as a link in the page.
I tried XSS and it  worked out. The awkward thing was that they never tried to filter
the url, when the request was being sent to their server. Then I reported it to Google.
May be they were in their winter break, so they took some time to respond. But atlast
they responded and google rewarded me with $1337.

<img class="image-center" src="http://i39.tinypic.com/2csewls.jpg" alt="XSS in Google"/>
