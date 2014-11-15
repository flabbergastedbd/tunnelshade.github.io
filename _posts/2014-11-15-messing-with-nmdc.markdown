---
layout: post
title: "Messing around using NMDC protocol"
categories: Hacks
tags: tricks hacks
---

### What is NMDC?

_________________

[**NeoModus Direct Connect**](http://en.wikipedia.org/wiki/NeoModus_Direct_Connect) was initially a file-sharing client for
Windows and Mac users that provided file-sharing capabilities for any type of file within a hub-centric, peer-to-peer network.
NeoModus Direct Connect inspired the creation of open-source versions of the client, such as Open Direct Connect and later DC++.
DC++ itself has several specialized spin-off open-source versions as well, such as ApexDC++ and StrongDC++, which are refined with
features particular to user groups, such as filters, language packs, and search capabilities.

### Why NMDC?

_________________

Inside my University the most popular way of sharing files is using these DC Hubs. Not only us, there are many hubs in local networks
of many educational institutions. For example, look at [this](http://dc.iitb.me) cool utility in **IIT-B**. There are many hubs on the
internet as well whose list is available [here](http://dchublist.org/nmdc-hubs). So, I was intrigued to understand this protocol
better to see how secure it is. Moreover no major analysis is available on the internet for me to go through.

### What did I do?

_________________

The best resources I have are [this](http://nmdc.sourceforge.net/NMDC.html) & [this](http://wiki.gusari.org/index.php?title=Main_Page).
Inorder to be able to test various stuff, I used python to write small snippets using socket library to try different stuff.

### Bugs/Hacks

_________________

The commands in the protocol start with a **'$'** sign and end with a **'|'**. The list of commands are available [here](http://nmdc.sourceforge.net/NMDC.html).
Since the analysis is currently in progress, I will try to update this section whenever I find something new. The hacks are classified
into three categories

+ Client Side - The bugs which are caused because of improper handling in client softwares
+ Hub Side - The bugs which are caused because of hub softwares
+ Protocol Side - The bugs which try to use the protocol in unexpected ways

### Client Side Hacks

_________________

#### User Spoofing in public/private chat

The command to send public and private messages are of the following format

```
$<john> cats are cute|
$To: john From: peter $<peter> dogs are more cute|
```

If you register your nick as **john>**, some clients strip out the trailing **>** and display the message which makes it appear as if
it is from **john**. The message commands will look like

```
$<john>> I really don't care about what you think|
$To: peter From: john> $<john>> You are an idiot|
```

Some clients on which I tested this are

+ StrongDC++ (<=2.42)
+ DC++ (<=0.843)
+ I am sure there will be many more...

### Protocol Side Hacks

_______________________

#### Spoofing Share Size

Since there is no practical way for the hub to verify your share size, you can spoof yours to attract people to browse your shared files.
This is not a huge attack on its own, but it has helped to draw the attention of users and make them download some malware :P. The command
in which share size is sent looks like [this](http://nmdc.sourceforge.net/NMDC.html#_myinfo).

```
$MyINFO $ALL johndoe <++ V:0.673,M:P,H:0/1/0,S:2>$ $LAN(T3)0x31$example@example.com$1234$|
```

#### Eavesdropping on users

The protocol is designed in such a way that whenever a user searches for a file, all the clients connected to that hub get
this search query. If you are using a dc client software, you will never know what others are searching since the clients automatically
send the search results by actually searching your shared files, but if you use a handcrafted client you can easily categorize all the
searches of a particular user. When the download slots for a file are not enough, the client software tries to search using the **TTH**
of the file. Again this search is sent to all the connected clients. It is easy to match a TTH to a file, by just searching it on the hub
itself ;). Though this is facilitated by the protocol itself, I find it amusing to see what other users are searching for. So, some
screenshots of search queries that are extracted from some online hubs :P are present [here](http://i.imgur.com/X3CPMZI.png),
[here](http://i.imgur.com/XsPTNL5.jpg) & [here](http://i.imgur.com/013VAZN.png).

<img src="http://i.imgur.com/X3CPMZI.png" class="image-center" style="height:500px" alt="IRC Screen"/>

<img src="http://i.imgur.com/kPNCDQY.png" class="image-center" style="height:500px" alt="IRC Screen"/>

Instead of the ip addresses we can easily track the searches using registered nicks on the hub. All the TTH searches are the files
that are being downloaded by users.

#### Spreading malware (Spoofing search results)

We have already seen that the hub sends the search query to every connected client for results. We can send fake results as there
is no way a hub can verify if we have a file or not. So, if the user wants to download one of our results, we can just send
malware. But this is not as simple as it seems. There are many factors which are to be considered. For example, search queries can be
of following [types](http://nmdc.sourceforge.net/NMDC.html#_search)

```
$Search 192.168.1.5:412 T?T?500000?1?Gentoo$2005|
$Search Hub:SomeNick T?T?500000?1?Gentoo$2005|
$Search 192.168.1.5:412 F?T?0?9?TTH:TO32WPD6AQE7VA7654HEAM5GKFQGIL7F2BEKFNA|
$Search Hub:SomeNick F?T?0?9?TTH:TO32WPD6AQE7VA7654HEAM5GKFQGIL7F2BEKFNA|
```

So, whenever you get one search query, you can send a **convincing** fake result. I wrote a small python script which sends a fake
result for every search :P

```
$SR MyNick F:_\search_term.proper_extension<0x05>file_size 5/5<0x05>HubName (192.168.1.1:411)|
```

But this is not as easy as it seems because our search results have to be convincing. Things to keep in mind

+ If the file is big and there are matching results, then the victim client will try to download part of file from other clients.
+ Using appropriate file extension when particular type of files are searched. Clients will filter results which donot match extension
+ Using a unique TTH hash for every different fake file :P
+ Using convincing file size because there cannot be a movie mp4 file of 25kB :P
+ Let me tell you it is more complicated to produce convincing results, best way is to cache the search results and extract useful
filenames and TTH hashes. This is tricky.

#### Just watching the world burn (Making downloads of all users useless)

There are few people who just want to watch the world burn. For them, there is a facility in NMDC. The clients search using TTH
hashes for alternate sources to download a part of file. If you respond to that search and send random binary data, the downloaded
file will end up being useless. Nothing particularly of interest, but will be fun to mess with few people. This feat will require huge
processing power and great bandwidth even for an active hub with users in two digits.
