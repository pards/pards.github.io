---
layout: post
title: List.clear() throws UnsupportedOperationException
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_title: List.clear() throws UnsupportedOperationException
  _aioseop_description: List.clear() throws UnsupportedOperationException
  _aioseop_keywords: java List clear, UnsupportedOperationException
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Consider the following code  
[code]  
String[] s = new String[]{""};  
List<String> list = Arrays.asList(s);  
list.clear();  
[/code]

All looks good, right? When you run it you get:  
[code]  
java.lang.UnsupportedOperationException  
at java.util.AbstractList.remove(AbstractList.java:144)  
at java.util.AbstractList$Itr.remove(AbstractList.java:360)  
at java.util.AbstractList.removeRange(AbstractList.java:559)  
at java.util.AbstractList.clear(AbstractList.java:217)  
[/code]

This is because Arrays.asList(array) returns a fixed-size list backed by the
specified array. Fixed-size being the operative word.

