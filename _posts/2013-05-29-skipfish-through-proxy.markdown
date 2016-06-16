---
layout: post
title: "Skipfish through a proxy"
categories: Hacks
tags: tricks tools
image: https://skipfish.googlecode.com/svn/trunk/assets/sf_name.png
---

My recent use of skipfish for benchmarking some proxies left me
searching for a way to route skipfish requests through a proxy server.
After searching the web for few frantic moments, I understood that there
are two approaches to solve this problem.

1. **To recompile skipfish after enabling the proxy feature** :-
    - Open **skipfish/src/config.h**
    - Uncomment line **67**. Now it should read as ***#define PROXY_SUPPORT 1***
    - Save the file.
    - Use the command **make **in the main folder to recompile skipfish.
    - Now you can provide a proxy address using **-J** ( check ./skipfish --help )

2. **The second way is to use burp or zap as proxifiers**. Follow the article present [here](http://vanstechelman.eu/security/using_skipfish_through_burpsuite).

EDIT :- If you get a pcre.h error then install libpcre3-dev
