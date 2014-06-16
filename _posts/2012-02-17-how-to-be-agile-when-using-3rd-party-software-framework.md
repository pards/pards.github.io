---
layout: post
title: How to be Agile when using 3rd party software framework
categories:
- Software Development
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

It can be difficult to be Agile when working with 3rd party software
framework.  The vendor product may dictate many aspects of the software
architecture thwarting your attempts at automated testing and continuous
build.

However there are steps you can take to make Agile easier.  I'll discuss a few
of the options that have worked for me on my current project.

  * Insulate yourself from static methods.
  * Introduce wrappers (Facades) to vendor classes.
  * Introduce Spring, with different contexts for testing vs production, to allow easy substitution of vendor classes.
  * Make use of mock objects for testing your code in isolation.
  * If possible, move responsibility into services that live outside the framework.

In our particular case we've done all of the above and have managed to build a
stable system with respectable test coverage.  We've also engaged the vendor
to see if they can modify some of the framework interfaces to simplify unit
testing, and they have been open to our suggestions.

