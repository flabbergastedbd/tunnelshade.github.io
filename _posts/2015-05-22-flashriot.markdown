---
layout: post
title: "Flashriot - Using Flashbang for bulk analysis"
categories: Hacks
tags: tools
image: https://pbs.twimg.com/profile_images/1884362265/phantomjs_400x400.png
---

I recently had the necessity to test multiple flash files for XSS. [Flashbang](https://cure53.de/flashbang)
is an awesome tool for this kind of work. Since Flashbang needs a browser to run, the only way to automate it for
multiple files is to use a headless browser like [PhantomJS](http://phantomjs.org). So, it was easy to build a
wrapper script as I know my way around Flashbang.

### Usage

The least amount of time for this whole tool was spent for deciding its name i.e 1 second, hence the misleading
name. The wrapper script is available at [github.com/tunnelshade/flashriot](https://github.com/tunnelshade/flashriot).
Some key things on using this tool:

+ PhantomJS, Python2 and Flashbang itself are necessary to run this. If you are on OSX, you can install phantomjs
using homebrew.
+ Run the shell script using proper arguments.

    ```bash
    ./flashriot.sh ~/workspace/flash-files/ ~/workspace/Flashbang/
    ```

+ Keep cheking the window periodically. Even if the script hangs, use **Ctrl+C** and start the script again. Flashriot
will only run flashbang on unprocessed files.
+ The output for each flash file is written to a text file with the same name. For a vulnerable **ZeroClipboard.swf**, we have
the following **ZeroClipboard.txt**.

    ```
	==> ZeroClipboard.txt <==
	========================
	Detected Flash variables
	========================
	id:eval
	width
	height
	========================
	===================
	Detected Sink calls
	===================
	---------------
	Flash variables
	---------------
	{"id":"someDummyPayloadWhichHasToBeThereInOnePiece","width":"4232","height":"2079"}
	----
	Sink
	----
	eval
	---------
	Sink Data
	---------
	try {__flash__toXML(ZeroClipboard.dispatch("someDummyPayloadWhichHasToBeThereInOnePiece","load",null));} catch (e) {"<undefined/>";}
	------------------
	Replaced Sink Data
	------------------
	try {__flash__toXML(ZeroClipboard.dispatch("@@@id@@@","load",null));} catch (e) {"<undefined/>";}
	-------------------
	Vulnerable variable
	-------------------
	id
	===================
    ```

+ After completion, just skim through the file outputs. I use the following command to see the file specific output.

     ```bash
     tail -n +1 *.txt
     ```

+ Sometimes, it is benificial to see the console output as well. For example from the following console output we can understand that
some kind of request was done and parsing failed. This mostly happens when a valid xml file is not supplied which might be due to an
improper path in a flash variable. Flashbang is not yet smart enough to detect a XML file call and fuzz using a XML file. If implemented,
this will be awesome.

    ```bash
	Going to fuzz /Users/tunnelshade/workspace/src/github.com/cure53/Flashbang/flash-files/files/tagcloud.swf
	Using url: http://127.0.0.1:9000/tagcloud.swf
	------------ Launching new instance ------------
	SyntaxError in ToXML

	  http://127.0.0.1:9001/shumway/src/avm2/xml.js:506 in toXML
	  http://127.0.0.1:9001/shumway/src/avm2/xml.js:717 in ASXML
	  http://127.0.0.1:9001/shumway/src/avm2/xml.js:712 in ASXML
	  http://127.0.0.1:9001/shumway/src/avm2/domain.js:266 in apply
	  http://127.0.0.1:9001/shumway/src/avm2/runtime.js:498 in asCallProperty
	  :7 in fn251
	  http://127.0.0.1:9001/shumway/src/avm2/scope.js:178 in boundMethod
	  http://127.0.0.1:9001/shumway/src/flash/events/EventDispatcher.js:101 in processListeners
	  http://127.0.0.1:9001/shumway/src/flash/events/EventDispatcher.js:79 in doDispatchEvent
	  http://127.0.0.1:9001/shumway/src/flash/events/EventDispatcher.js:252 in dispatchEventFunction
	  http://127.0.0.1:9001/shumway/src/avm2/runtime.js:498 in asCallProperty
	  :13 in EventDispatcher$$BgdispatchEvent
	  http://127.0.0.1:9001/shumway/src/avm2/runtime.js:498 in asCallProperty
	  :6 in URLLoader$$G_muXFIonStreamComplete
	  :0 in URLLoader$$G_muXFIonStreamComplete
	  http://127.0.0.1:9001/shumway/src/flash/events/EventDispatcher.js:133 in processListeners
	  http://127.0.0.1:9001/shumway/src/flash/events/EventDispatcher.js:79 in doDispatchEvent
	  http://127.0.0.1:9001/shumway/src/flash/events/EventDispatcher.js:223 in dispatchEvent
	  http://127.0.0.1:9001/shumway/src/flash/net/URLStream.js:92 in onclose
	  http://127.0.0.1:9001/shumway/examples/inspector/js/classes/BinaryFileReader.js:79 in onreadystatechange
	Done, now proceeding to clean up
	------------ Killing used instance ------------
	------------ Launching new instance ------------
	Done, now proceeding to clean up
	------------ Killing used instance ------------
	------------ Launching new instance ------------
	Done, now proceeding to clean up
	------------ Killing used instance ------------
    ```

### Bug Hunters!

+ When bug hunting, search for all flash files for the domains. Either use google dorking (**site: ext:swf**) or
[thedumpster](https://github.com/tunnelshade/thedumpster) to gather all flash file urls.
+ Then use **wget** or similar tool to download them into a folder.
+ Run **Flashriot** on those files.
+ **BOOM!!** for any bugs you might find.

	```bash
	$ mkdir hunting; cd hunting;
	$ python3 thedumpster.py -a "ext:swf" -o urls.txt swag_seller.com
	$ wget -i urls.txt
	$ sh flashriot.sh ./ ~/workspace/Flashbang
	$ tail -n +1 *.txt
	```
