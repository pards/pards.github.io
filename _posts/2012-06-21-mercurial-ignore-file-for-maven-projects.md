---
layout: post
title: Mercurial ignore file for Maven projects
categories: 
- Version Control
tags: 
- Mercurial
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
    

