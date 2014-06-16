---
layout: post
title: Eclipse can't find source when remote debugging
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_keywords: eclipse remote debug source, eclipse remote debugging, eclipse
    remote debugging cannot find source files
  _aioseop_description: Eclipse can't find my source files when remote debugging.
    I had to add the source to the remote debug configuration (run -> debug configurations...
    -> source tab)
  _aioseop_title: Eclipse can't find source when remote debugging
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I had a problem recently where Eclipse couldn't find my source files when
remote debugging a particular application. It would stop and the breakpoint
and show the class file with a "attach source" button, but pointing it to the
source directory didn't do anything.

It turns out that the solution was to add the project to the remote debug
configuration.

This is done by "Run -> Debug configurations..."  
Choose the remote config from the tree on the left  
Click on the "Source" tab  
Click on the "Add..." button  
Follow the wizard.

Hope this helps anyone with the same problem.

