---
layout: post
title: "Firefox addons for hackers"
categories: information
tags: tools
---

The following is a list of addons that
 I find extremely useful when searching for security loopholes in web 
applications using firefox.

- [Hackbar](https://addons.mozilla.org/en-US/firefox/addon/hackbar/)

This addon allows us to calculate common types of hashes and provides us
 with some common encryption methods. It also allows us to edit post 
data and referrer on fly.

- [Tamper Data](https://addons.mozilla.org/en-US/firefox/addon/tamper-data/?src=search)

This addon becomes your best friend when it comes to editing POST or GET
 requests. This particular addon eliminates the use of intercept proxies
 when the job is to manually modify the request headers and post data. 
It also displays the time taken for a response to come from webserver, 
which is helpful in Blind SQLi.

- [Live HTTP Headers](https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/?src=search)

  

This addon is another utility to play with the headers. It allows us to 
repeat HTTP requests by slightly modifying the parameters.

- [Firebug](https://addons.mozilla.org/en-US/firefox/addon/firebug/?src=search)

  

The most popular addon among web developers. It allows you to edit HTML 
code in your browser, provides interface for playing with cookies, js 
console and many more features. It is better to add Flash firebug also 
which gives the capability to edit AS3 files in the browser itself.

- [Foxyproxy](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/?src=search)

  

Though this addon doesn't provide any active service in vulnerability 
assessment, it helps a lot in a passive manner. It provides an easy 
interface for switching between different proxies. We can also whitelist
 and blacklist urls. It even has the facility to automatically switch 
between proxies based on the predefined url matchings.  
  

  

Even though my list officially ends here, I would like to add two addons provided by Security Compass for firefox. One is **SQL inject me** and the other one is **XSS Me**. Their functionality is very clear from their names.

  

UPDATE - I recently came across another addon called&nbsp; **Fireforce** which helps in bruteforcing web application forms.