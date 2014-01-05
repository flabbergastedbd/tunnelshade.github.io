---
layout: post
title: "Signing an SSL cert using PyOpenSSL"
categories: programming
tags: tricks python
---

While I was doing my GSOC 13 project, I came across a situation which involves
generation and signing of certificates on fly using a custom CA. So I searched
the web and could only find outdated examples. I decided to share my script if
anyone wishes to get some kind of kickstart. I added comments as per peoples' requests :P

{% gist 5779960 %}
