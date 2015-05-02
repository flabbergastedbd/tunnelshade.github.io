---
layout: post
title: "Crawler for NMDC networks"
categories: Python
tags: tools python
image: http://www.kazemjahanbakhsh.com/figs/crawler.jpg
---

#### Crawler

What is the meaning of a crawler in a file sharing network? Very simple for me. Something that collects all
the files that are shared by clients on that dc network. But collecting all the shared files is a huge task. So, I
reduced my task to just collecting metadata and organising that information. Metadata about shared files in a dc
network is nothing but the file lists that the client softwares create. The main point of crawling is for stats.

{% highlight xml %}
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<FileListing Version="1" CID="PQ5QETB5G44DH36XGW7INMWHPH6WDDX5KUGCCIY" Base="/" Generator="DC++ 0.843">
	<Directory Name="F:_">
		<Directory Name="fimap_alpha_v09">
			<Directory Name="config">
				<File Name="generic.xml" Size="2235" TTH="6BS6CZ2DQJ6LFZYQCKBC6C5GUHSVLPCCJCTM7BQ"/>
				<File Name="perl.xml" Size="1685" TTH="JPHFI42ZEPPT4JM772H2Q4W2PDKKTWA4TAE3NDI"/>
				<File Name="php.xml" Size="7835" TTH="S7WKKYRMANXTMEP4HCIKM6VNDVLSQTYD5EZHEBY"/>
			</Directory>
			<Directory Name="doc">
				<File Name="AUTHORS" Size="345" TTH="MJ4TIIXYJZF57ZHNSTECX3OZPVX3CQD7AXHAR2Y"/>
				<File Name="LICENSE" Size="17987" TTH="MRQYN544QVRPQIB4ZPYDXYGP6KSBZUI2V2J3HHY"/>
				<File Name="THX" Size="2642" TTH="PSCYZ37EZYTONNWROEBIMFXEQ3ZIDUQZUIUK4NA"/>
			</Directory>
			<Directory Name="xgoogle">
				<File Name="__init__.py" Size="1354" TTH="7MGPO5Y5SKPOI3FI2Z4QSJNA3PBSXPLOJBIGWHA"/>
				<File Name="browser.py" Size="4543" TTH="DWSYNHG46ROVBFFLBM5MO72IMBKXYVTPZDIROHQ"/>
				<File Name="sponsoredlinks.py" Size="7999" TTH="EIG7R35X7DAON6INPYBO5OSLCQELVOOSCE4TYHI"/>
			</Directory>
			<File Name="crawler.py" Size="5873" TTH="6WVCYHBICDJWFRONNTPKB4FUBG4FU2FVDJGQUFQ"/>
			<File Name="fimap.py" Size="32569" TTH="TFWMQYA3OAYTJTXIFDMCBGTNX7RUGVZXBTSOESI"/>
		</Directory>
		<File Name="Andrew Ng- Deep Learning.mp4" Size="244082418" TTH="ZDRCQLMCUCBAN2MTN3NB3V5FHSERQY3XQNPO6KA"/>
	</Directory>
</FileListing>
{% endhighlight %}

Since a db is needed to store all the data, I decided to use **PostgreSQL**. The reason being that I had some experience with
it as we use the same in OWTF. Writing SQL queries is a pain for this pet project, so I decided to use **SQLAlchemy ORM**.

#### Models

There are only two models for this crawler.

  + User
  + File

Both are joined by a many to many relationship as same file can be with multiple users and a user will have multiple files.

#### Configuration

The crawler is [here](https://github.com/tunnelshade/dcfury/blob/master/nmdc.py). The db settings are present to the end of
the file.

{% highlight python %}
db_settings["DATABASE_IP"] = "127.0.0.1"
db_settings["DATABASE_PORT"] = "5432"
db_settings["DATABASE_NAME"] = "dcrawl"
db_settings["DATABASE_USER"] = "crawl_bot"
db_settings["DATABASE_PASS"] = "crawl_bot"
{% endhighlight %}

#### Usage

+ Get the crawler script, better if you get the whole [repo](https://github.com/tunnelshade/dcfury)
+ Configure the database settings, i.e create the db role and db if necessary. There is a helper script called *db_setup.sh* script
in the repo
+ Since this was an experimental project, if you wish to listen/crawl a particular hub, you have to edit the crawler. Find the following
line and change the ip and port to the desired value.

{% highlight python %}
  hub = NMDCClient("10.8.20.21", 4112, db_settings)
{% endhighlight %}

+ Now, you are ready. Use one of the following commands

{% highlight bash %}
python2 nmdc.py listen # Will connect to a hub and just be in listen mode i.e will keep printing to screen what it gets
python2 nmdc.py collect # Will collect file lists from the users and place the xml files in a folder named file_lists
python2 nmdc.py analyse # Will parse the collected files in the file_lists folder and add them to the db with proper relationships with the user
{% endhighlight %}

#### Working

+ See the source code :P

#### Demos

--------------------------------------------------------------------------------------------------------------------------

<iframe width="640" height="480" src="https://www.youtube.com/embed/o7UbuifCXpI" frameborder="0" allowfullscreen></iframe>

--------------------------------------------------------------------------------------------------------------------------

<iframe width="640" height="480" src="https://www.youtube.com/embed/rgcNEaU4kJs" frameborder="0" allowfullscreen></iframe>

--------------------------------------------------------------------------------------------------------------------------

<iframe width="640" height="480" src="https://www.youtube.com/embed/AB4Tj_EkDrQ" frameborder="0" allowfullscreen></iframe>
