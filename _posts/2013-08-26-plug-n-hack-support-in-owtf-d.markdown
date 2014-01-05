---
layout: post
title: "Plug-n-Hack support in OWTF"
categories: information
tags: owtf
image: https://www.owasp.org/images/0/02/OWTFLogoSmall.png
---

**Plug-n-Hack** (PnH) is a proposed standard from the Mozilla security team 
for defining how security tools can interact with browsers in a more 
useful and usable way. More info about PnH can be found in this [blog post](https://blog.mozilla.org/security/2013/08/22/plug-n-hack/).  
  

The interesting thing is that **OWTF** now supports this standard and below you can find a mini guide on getting things going.  
  

1. Currently Firefox supports PnH through **FxPnH add-on**, so you must have it installed( Add-on can be downloaded from [here](https://github.com/mozmark/ringleader) ).
2. Make sure you have the latest copy of OWTF ;) , start owtf along with **--proxy flag**.
3. Fire up your **firefox** & check the console for PnH configuration link.
4. Click on the setup link in the console.
![PnH Support](http://i41.tinypic.com/w20jn4.jpg)
5. You can revert back the proxy settings using this command in **gcli** (Shift + F2)
{% highlight bash %}
pnh config clear
{% endhighlight %}
