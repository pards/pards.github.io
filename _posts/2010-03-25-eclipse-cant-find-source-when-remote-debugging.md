---
layout: post
title: Eclipse can't find source when remote debugging
categories:
- IDE
tags: [Eclipse]
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

