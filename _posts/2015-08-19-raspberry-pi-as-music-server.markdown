---
layout: post
title: "RPi 2 as my music server"
categories: Linux
tags: tools
image: https://www.raspberrypi.org/wp-content/uploads/2015/02/Pi_2_Model_B.png
---

### Introduction

Almost everyone is familiar with what [Raspberry Pi](https://www.raspberrypi.org) is, if you are not aware of it better stop reading! When Pi2 was
[released](https://www.raspberrypi.org/blog/raspberry-pi-2-on-sale/) with some great upgrades I got one to just tinker with it. Couldn't do much with it until recently. I joined my first full time role at a
company, so I spend approx. 6 hours of a weekday at office.

I have my work laptop and then my personal MBPR, syncing music between these is a pain. Sometimes I just want to lay back and browse through my music
collection(AFK time). I have a 2.1 speaker set which I usually connect to my laptop when I wish to listen to some music (All the time!)

### Things I have

+ **2.1 Speaker System**
+ **Raspberry Pi 2**
+ Home WiFi Network

### Solution

[**MPD**](http://www.musicpd.org/) or Music Player Daemon is a best fit solution for my problem. All my music will be on some kind of memory storage accessible to pi2. If I run
mpd on the pi2 connect my speakers to it and I can just connect to it using [**NCMPCPP**](http://ncmpcpp.rybczak.net/) on my laptops while in my home network. If I am at office,
I can still stream my music collection to my office laptop over a vpn. One good side effect is that there are good mpd client android applications like [**MPDroid**](https://play.google.com/store/apps/details?id=com.namelessdev.mpdroid&hl=en). This way, I have
only one place to keep my songs and I can enjoy them everywhere.

The following is a reminder for me as getting MPD to run on Pi2 is not so straightforward. I use a [Kali Linux 2.0 image](https://www.offensive-security.com/kali-linux-vmware-arm-image-download/) on Pi.

+ Make sure you connect your Pi in your home network with a static IP. This will allow you to save settings in your mpd clients.
+ I use Kali as a non-root user, so create an account for it.
+ Sound is disabled by default on Pi2. Install `alsa-utils` if not present and load `snd_bmc2835` module.

	```
	# apt-get install alsa-utils
	# modprobe snd_bcm2835
	```

+ To load this module everytime at boot, just add it to `/etc/modules`.

	```
	# echo "snd_bcm2835" >> /etc/modules
	```

+ I want to run MPD under the non-root user, so this user has to be added to the `audio` group if not already present.

	```
	# grep "audio" /etc/groups
	# usermod -a -G group user
	```

+ Now, hack up the configuration of mpd. Default is present at `/etc/mpd.conf`. I prefer using it from my home directory, so copied it and edited it.
All the sections of self explainatory and I am pasting here a minified version. Make sure you create all the files and folder mentioned here and ensure
that MPD has access to write to these locations and your music directory

	```
	music_directory		"/home/tunnelshade/Music"
	playlist_directory		"/home/tunnelshade/Music/playlists"
	db_file			"/home/tunnelshade/Music/mpd/tag_cache"
	log_file			"/home/tunnelshade/Music/mpd/mpd.log"
	pid_file			"/home/tunnelshade/Music/mpd/pid"
	state_file			"/home/tunnelshade/Music/mpd/state"
	sticker_file                   "/home/tunnelshade/Music/mpd/sticker.sql"
	user				"tunnelshade"

	bind_to_address		"0.0.0.0"

	port				"7000"

	password                        "password@read,add,control,admin"

	input {
		plugin "curl"
	}

	audio_output {
		type		"alsa"
		name		"My ALSA Device"
	}

	filesystem_charset		"UTF-8"
	id3v1_encoding			"UTF-8"
	```

+ And lastly, have to start MPD at every boot. If we add a service then it will start with system privilege, but there is one alternative. Add it as
a user cron after start. So, for me the cron entry will look like (``crontab -e``)

	```
	@reboot mpd ~/.mpdconf
	```

+ Just reboot and connect from your favourite devices.

### Screenshots

<iframe src="https://drive.google.com/embeddedfolderview?id=0B7bSDtYJAnbPflZwbFBCYUl0eWlFOTYzLV93Y0hfaWdSeWZmcGEyN01LY1JsYmFRaUJxYzA#grid" width="100%" height="500px" frameborder="0"></iframe>
