---
layout: post
title: How to show which .hgrc Mercurial is using
categories:
- Version Control
tags: 
- Mercurial
---

Mercurial loads a bunch of different configuration files and overlays their
values on top of each other. The [hgrc man
page](http://www.selenic.com/mercurial/hgrc.5.html) shows the order in which
these files are loaded.

I encountered an issue recently where I knew that hg was using an erroneous
setting but I had no idea where the setting was coming from.

The magic answer is  

	hg --debug showconfig  

This will output all the current settings, along with the name of the file
that it got each setting from. Very useful indeed.

