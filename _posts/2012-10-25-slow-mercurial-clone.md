---
layout: post
title: Slow Mercurial clone
categories:
- Version Control
tags: 
- Mercurial
---

I recently set up a Mercurial server (Windows, Apache, ActiveDirectory and
hgweb.wsgi). However, cloning our 1Gb repository took an hour or more and
often failed.

The only information in the Apache log was  

	mod_wsgi (pid=1234: Exception occurred processing WSGI script
	'C:/hg/hgweb/hgweb.wsgi'  
	IOError: failed to write data  

The solution was to disable compression by adding the following lines to
hgweb.config  

	[server]  
	preferuncompressed = true  

By default, Mercurial will compress everything before sending it down the
pipe. This makes sense if you have a powerful server and a slow network, but
the opposite was true for us - Mercurial's hosted on an old dual-core Windows
VM connected to a 1Gb corporate LAN.

Cloning takes under 5 mintues after the configuration change.

