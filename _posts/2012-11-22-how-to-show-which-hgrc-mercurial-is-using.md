---
layout: post
title: How to show which .hgrc Mercurial is using
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  Title: How to show which .hgrc Mercurial is using
  _syntaxhighlighter_encoded: '1'
  _aioseop_keywords: mercurial, hgrc
  _aioseop_title: How to show which .hgrc Mercurial is using
  _edit_last: '2'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Mercurial loads a bunch of different configuration files and overlays their
values on top of each other. The [hgrc man
page](http://www.selenic.com/mercurial/hgrc.5.html) shows the order in which
these files are loaded.

I encountered an issue recently where I knew that hg was using an erroneous
setting but I had no idea where the setting was coming from.

The magic answer is  
[code]  
hg --debug showconfig  
[/code]

This will output all the current settings, along with the name of the file
that it got each setting from. Very useful indeed.

