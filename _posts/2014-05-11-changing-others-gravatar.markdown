---
layout: post
title: "Changing Gravatar of Others!!"
categories: Hacks
tags: tricks hacks
image: http://upload.wikimedia.org/wikipedia/commons/b/b2/Logo_Gravatar.png
---

So, yeah the title is true. I found some vulnerabilities which can be
chained to change the **gravatar** of any **logged-in user**. The one draw back for
this attack is the knowledge of the email address of the victim. I walked
through the process of changing gravatar and found a **CSRF** at every stage
except the last one. So I shifted to using **Clickjacking** for the last stage :P

{% gist tunnelshade/fcd818734c44e9605822 %}

#### **Timeline :**

19 Oct, 2013 - Vulnerabilities Discovered

19 Oct, 2013 - Tried contacting Automattic (maintainer of Gravatar)

11 Nov, 2013 - Got a response, so sent the POC

19 Feb, 2014 - Tried contacting regarding the status

21 Feb, 2014 - Status recvd. that some vulns were fixed

11 May, 2014 - Public disclosure

**P.S** - Got some swag from Automattic for these reports. They don't have a
bounty program, atleast they don't even have an email address to report vulns
in Gravatar.
