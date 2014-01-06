---
layout: post
title: "Way to fuzz (I learnt it this way :P)"
categories: information
tags: tricks info
image: http://peachfuzzer.com/images/peach_fuzzer.png
---

I had a hard time learning how to fuzz desktop applications and few web 
server softwares, because I cannot use google effectively. So I decided 
to compile a list for myself.  
  

Read this [book](http://www.amazon.com/gp/product/0321446119) if possible, it is super awesome :D  

- Best win32 exploit development tutorials - [corelan](https://www.corelan.be/index.php/2009/07/19/exploit-writing-tutorial-part-1-stack-based-overflows/)
 ( Because there is no fun in fuzzing when you don't know how to develop
 an exploit, unless you wish to do it to report some bugs :P )
- The fuzzers (or) fuzzer frameworks I used :- [peach](http://peachfuzzer.com/), [spike](http://www.immunitysec.com/resources-freesoftware.shtml), [uniofuzz](http://nullsecurity.net/tools/fuzzer/uniofuzz.py), [sickfuzz](https://code.google.com/p/sickfuzz/).
- I preffered peach because I like the idea of models.
- Best tutorials online for using peach fuzzer (or) making peach pit files :-&nbsp;

1. [http://peachfuzzer.com/v3/PeachQuickStart.html](http://peachfuzzer.com/v3/PeachQuickStart.html)
2. [http://www.flinkd.org/2011/07/fuzzing-with-peach-part-1/ ](http://www.flinkd.org/2011/07/fuzzing-with-peach-part-1/)
3. [http://peachfuzzer.com/v3/PeachPit.html](http://peachfuzzer.com/v3/PeachPit.html)
4. [http://www.nullthreat.net/2011/01/fuzzing-with-peach-install-part-1.html ](http://www.nullthreat.net/2011/01/fuzzing-with-peach-install-part-1.html)