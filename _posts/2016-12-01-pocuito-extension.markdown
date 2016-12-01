---
layout: post
title: "Pocuito - A little web extension"
categories: Tools
tags: extension
image: "https://cdn.rawgit.com/tunnelshade/pocuito/master/images/icons/icon128.png"
---
<br>

### Requirement
---------------

One of the main issues people face in web application based organizations is the **channel of communication between the
security team and developers**. This often involves **lengthy steps for reproduction of vulnerabilities**. Often times
these are tedious to follow let alone repeat multiple times for the pentesters. So, I was behind a solution to this problem.
[Mozilla ZEST](https://blog.mozilla.org/security/2014/01/20/reporting-web-vulnerabilities-to-mozilla-using-zest/) is something
that I have known for a while. **ZEST** allows to replay requests and some assert statements. The serious _limitation of this
approach is in modern day web applications which need complex user interactions on the interface_.

### Solution
----------------

To encounter the above limitation I assumed the logical way is to use **a browser extension to record/replay user
interactions like clicks and input fills**. The stringent rules on web extensions in chrome don't allow you to read/tamper
requests. Hence, the hybrid solution is to use a proxy and write an extension that makes use of that proxy to tamper & record
required requests.

### Pocuito
---------------

Pocuito is a two part solution.

+ Extension that you install on your browser.
+ Proxy server which can be deployed for multiple users inside your org network.

**NOTE: The proxy maintains session based on the ``remote_ip`` it sees the request from, hence use it from local network**

### Download
----------------

You can download the extension & proxy on [Github Releases](https://github.com/tunnelshade/pocuito/releases).

### Usage
----------------

I highly suggest going through the [README](https://github.com/tunnelshade/pocuito) which will stay updated all the time.
