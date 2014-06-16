---
layout: post
title: JRebel and Tapestry working together
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_keywords: JRebel, Hot-Swap, JRebel Tapestry
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I've recently started trialling
[JRebel](http://www.zeroturnaround.com/jrebel/) at work. JRebel is a piece of
software that hot-swaps your code so that changes made in your IDE are
reflected in your application server _without_ requiring a restart.

The benefits of this idea are fairly obvious. I no longer have to wait 1 - 2
minutes for JBoss to restart each time I correct a spelling mistake etc. The
time saved by not restarting probably make the product worth the $US149
licensing fee.

Then there is the significant reduction in [context
switching](http://www.joelonsoftware.com/articles/fog0000000022.html). I no
longer make a code change, restart JBoss, read the New York Times for 2
minutes, forget what I changed, then try to test it. Instead I make the change
and test it straight away. This can only mean fewer bugs (and perhaps a less-
informed developer but people assume that nerds live under rocks anyway, so
that's ok).

Once JRebel was installed my JBoss startup time went from 1:40 to 3:45, but I
figured that would be OK because I would only have to do that once a day.
JRebel did most of what I expected it to, and I was happily coding away in my
new no-context-switching life until I changed a Tapestry binding and the code
wasn't hot-swapped even though I had the Tapestry plug-in enabled.

This forced a JBoss restart (or several, to be honest) and I had lost all the
benefit of JRebel. Later I discovered that I needed to combine a few things to
get JRebel to work nicely with Tapestry. Here's what I had to do:

  1. Enable the Tapestry 4 plugin (-Drebel.tapestry4_plugin=true)
  2. Enable Tapestry reset service (-Dorg.apache.tapestry.enable-reset-service=true)

Now whenever I change a Tapestry file, I simply invoke the Tapestry reset
service (http://localhost:8080/myApp/app?service=reset) and then refresh the
page. Voila! The code gets hot-swapped.

I think this technique works because invoking Tapestry's reset service clears
its cache and forces it to reload the classes which have been updated by
JRebel.

