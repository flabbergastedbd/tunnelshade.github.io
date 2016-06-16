---
layout: post
title: "Stegosploit is simple fun!!"
categories: Python
tags: tools
image: https://i.imgur.com/tiEFZbc.png
---

#### Backdrop

If you are not familiar with the word **STEGOSPLOIT** then you must definitely look at the
following links

+ [The actual talk](https://conference.hitb.org/hitbsecconf2015ams/sessions/stegosploit-hacking-with-pictures/)
+ Followed by huge popularity. Just google the word!
+ Then [criticism](https://medium.com/@christianbundy/why-stegosploit-isn-t-an-exploit-189b0b5261eb)
+ Then, I stopped following the topic at this stage.

Slides of the talk are available [here](https://conference.hitb.org/hitbsecconf2015ams/wp-content/uploads/2015/02/D1T1-Saumil-Shah-Stegosploit-Hacking-with-Pictures.pdf)

#### My Thoughts

+ Would love to have a tool that supports all image types. Other than that, embedding html in image metadata is common.
+ Non tech people who didn't think of what is happening in the background started giving out blunt statements like *PICTURES ARE NO MORE SAFE*. And
every other noob went crazy.

#### What counts?

The mention of *lcamtuf* in the slides made me google his **JPG+HTML** [polyglot](http://lcamtuf.coredump.cx/squirrel/). After going through the slides
of Mr.Shah, I decided to write a simple PoC with the exact steps taken from his slides. So, my innocent plan was to

+ Hide an exploit payload in the image pixels.
+ Create a **HTML+PNG** polyglot so the image itself can be used as a loader.
+ Then use **HTML5** canvas to reconstruct the actual payload in the browser.

For anyone who read those slides all this would seem normal & yes it is!!

#### Script

{% gist tunnelshade/757de16b6ac6f5f337fd %}

#### How it works??

+ First it takes the payload file and converts the content into a bit string.
+ This bit string is hidden inside **LSB** bit of R, G & B pixel values of the input PNG file. This is done using [Pillow](https://pypi.python.org/pypi/Pillow/2.8.2).
+ Then the HTML requried to decode this payload is added to the same PNG file to create a HTML+PNG polyglot.
+ For the final PNG to deliver the payload, it should be served with content type **text/html**.
+ Then when image is loaded, the browser execute the HTML in it leading to reconstruction and running of exploit.

#### Selling points

+ Common users are aware of malicious websites but not malicious images yet ;)
+ As an end user, what difference do you see?

![Innocent cat](http://i.imgur.com/PCktIck.jpg)

+ Look again :P

![Innocent cat becomes evil](http://i.imgur.com/TcjV8yu.jpg)

#### Difficulties

+ Who servers images with html content type? Unless you want to pwn users!
+ ML trained detectors can catch these images, but you can always improvise ;)

**PS: The script only works on some PNGs for now**

#### Sample Console Output

```bash
.-[tunnelshade@MacBook-Pro.local:~/workspace/misc/poly]
'->$ python2 convert.py -i cat.png -p payload.html -o cute_kitty.png
[*] Opening payload and converting to bit string
[*] Hiding data in LSB
[*] Saving intermediate PNG
[*] Opening intermediate png for adding loader
[*] Writing PNG header
[*] Writing IHDR chunk
[*] Minifying loader html
[*] Writing iTXt chunk containing loader
[*] Writing the remaining data
```

#### Sample Polyglot with alert() payload

![Innocent cat becomes evil](http://i.imgur.com/tiEFZbc.png)

#### Serving the Polyglot

The image has to be served with a **text/html** content type. If not, the browser parser will
ignore the html part and just render the image. Below is a sample python script which when run in the
same directory as of the image, will serve the png with html content type.

```python
#!/usr/bin/python

import SimpleHTTPServer
import SocketServer

PORT = 8000

Handler = SimpleHTTPServer.SimpleHTTPRequestHandler
Handler.extensions_map.update({
    '.png': 'text/html',
});

httpd = SocketServer.TCPServer(("", PORT), Handler)

try:
    print "Serving at http://127.0.0.1:%d" % (PORT)
    httpd.serve_forever()
except KeyboardInterrupt:
    httpd.socket.close()
```
