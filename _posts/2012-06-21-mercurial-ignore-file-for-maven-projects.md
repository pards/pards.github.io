---
layout: post
title: Mercurial ignore file for Maven projects
categories: []
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I find it hard to remember all the .hgignore settings for a new Mercurial
repository that I need, so here's a bootstrap version

    
    
    
    syntax: glob
    .hgignore
    */target/**
    */.settings/**
    */.classpath
    */.project
    *.log
    *.orig
    

