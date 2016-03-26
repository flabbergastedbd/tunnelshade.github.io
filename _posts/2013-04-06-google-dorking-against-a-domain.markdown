---
layout: post
title: "thedumpster := Targeted Google dorking"
categories: Hacks
tags: tools screenshots
---

![thedumpster](http://i39.tinypic.com/28814d0.jpg)

####Installation

- Download the latest version from [here](https://github.com/tunnelshade/thedumpster)
- You need [python 3](http://python.org/) for running this tool.
- [PyQuery](https://pypi.python.org/pypi/pyquery) is also required for running this tool.

Pyquery can be installed using pip
{% highlight bash %}
sudo pip install pyquery
{% endhighlight %}

####Configuration

- First the proxies must be configured in the config file correctly.
Pick some open proxies which use basic http auth or no auth at all. The **username:password** part must be omitted if not required. Make sure you use only one proxy on one line.

{% highlight bash %}
username:password@proxy-ip:port
{% endhighlight %}

- Now just go the directory where thedumpster.py exists and run

{% highlight bash %}
python thedumpster.py --help
{% endhighlight %}

####Usage

{% highlight bash %}
usage: thedumpster.py [-h] [-l LIMIT] [-ghdb] [-ap] [-p] [-a ADD] [-r REM] [-ws] domain
{% endhighlight %}

####Flags / Parameters

- **-l** : The value provided here will be limit for the number of results that will be returned for your search.
- **-ghdb** : This flag when sets prompts you to select a type of dork from GHDB dorks, which will be tested against the domain.
- **-ap** : This flag when set asks for the possbile backend of the
 web infrastructure. Then based on the backend it picks appropriate
dorks for searching.
- **-p** : This flag allows to search for any pastebin dumps.
- **-a** : This argument allows you to add custom search keywords. Multiple search words must be separated by comma. Eg :- **intext:error,ext:sql**
- **-r** : This argument allows you to exclude search results of a
site. Multiple domain exclusion can be done by separating those domains
with a comma. Eg:- **foo.bar.com,shut.bar.com**
- **-ws** : The most important flag of all. This flag allows you to
 do google search which is against Google's TOS. So author is not
responsible for your usage.
- **domain** : This is a compulsory positional argument. Better to
provide the root domain so that all subdomains are included in search
results.

For example if we wish to search a site **example.com** , for **admin panels** while excluding sites **foo.example.com**, and extra query **intext:login** then you can use something like this

{% highlight bash %}
python thedumpster.py -l 10 -ap -a intext:login -r foo.example.com example.com
{% endhighlight %}

**P.S** The -ws flag is not taken by default
