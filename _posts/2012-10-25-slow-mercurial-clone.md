---
layout: post
title: Slow Mercurial clone
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  Title: Slow Mercurial clone
  _syntaxhighlighter_encoded: '1'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I recently set up a Mercurial server (Windows, Apache, ActiveDirectory and
hgweb.wsgi). However, cloning our 1Gb repository took an hour or more and
often failed.

The only information in the Apache log was  
[java]  
mod_wsgi (pid=1234: Exception occurred processing WSGI script
'C:/hg/hgweb/hgweb.wsgi'  
IOError: failed to write data  
[/java]

The solution was to disable compression by adding the following lines to
hgweb.config  
[java]  
[server]  
preferuncompressed = true  
[/java]

By default, Mercurial will compress everything before sending it down the
pipe. This makes sense if you have a powerful server and a slow network, but
the opposite was true for us - Mercurial's hosted on an old dual-core Windows
VM connected to a 1Gb corporate LAN.

Cloning takes under 5 mintues after the configuration change.

