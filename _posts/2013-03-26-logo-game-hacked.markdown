---
layout: post
title: "Facebook games are fun .... to hack!"
categories: Hacks
tags: tricks hacks
image: https://i43.tinypic.com/2wggoht.jpg
---

This is about how I learnt about logos with the help of a facebook game named **The Logo Game**.
During vacation because of extreme boredom in holidays, I finally started to look for some time
consuming stuff :P. Eventually I stumbled upon Facebook games in particular a game called The Logo Game.

<img src="http://i43.tinypic.com/2wggoht.jpg" class="image-center" alt="Logo Game"/>

The game was really cool (at the beginning). But the awkward thing that I noticed is that,
if a correct guess was provided it was taking time to respond & for a wrong guess the
reaction was instantaneous. So, it is reasonable to open tamper data and check for the
data submits. For a correct solution there was a request being done and for wrong as
expected no request was taking place. So I started diving into the source code :). What
caught my attention was ==>

<img src="http://i40.tinypic.com/8yyrrb.jpg" class="image-center" alt="MD5 Salt"/>

md5_salt :o ,  so I could guess what was going in the backend. So it was easy for me to search the remaining js code for relavent part & I found this

<img src="http://i40.tinypic.com/o7rpro.jpg" class="image-center" alt="Guess Function"/>

So I went for logos.CheckGuess

<img src="http://i41.tinypic.com/2co4zlu.jpg" class="image-center" alt="Check Function"/>

Yay!!! We got it, so some hashes were being passed to app, and checking is done in our browser itself (The app was combining the answer with a salt and then hashing it and checking the hash against the existing values ) . So now we have salt and hashes. Are we going to crack it?????
NO WAY. When I tried to replicate the application's way of communicating with the server through ajax, I got the some data in json format. I tried fuzzing the input parameters and I could get the whole data of logos and hashes ( also those which I don't have access as my score is not so good ). So the data after a bit of **injections** appeared like this =>

<img src="http://i42.tinypic.com/axd8co.jpg" width="100%" class="image-center" alt="Fuzzing result"/>

I just underlined few of the data because almost all the data is important. We have hints, md5 hashes and names of the logos. So I wrote a python script to extract logo names from this trash and the output of my program appeared like =>

<img src="http://i43.tinypic.com/51t4du.jpg" class="image-center" alt="Check Function"/>

Just a snapshot of a part of the output file. So now I have all the logo names in order. The file is available for download [here](http://www.mediafire.com/view/?wygcjzqi8auyopx). Finally there are loads of ways to learn stuff. So I started learning about logos this way.

P.S - The score that I have in the game is legible and is not obtained through any unfair means :P

Anyone can download the logos list and verify. Incase of mistakes please let me know as I have to correct my script then :P. Trust me all this was fun rather than the normal game.
