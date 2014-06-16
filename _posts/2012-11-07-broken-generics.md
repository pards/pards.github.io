---
layout: post
title: Broken Generics
categories: []
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Given the following code  
[java]  
Map<String, String> map = new HashMap<String, String>();  
map.put("123", "value");

Integer key = Integer.valueOf(123);  
String actual = map.get(key);  
[/java]

What is the value of "actual"?

  1. Compilation error - key should be a String
  2. "value"
  3. null

Answer: null

This blew me away. The whole point of type-safe collections was supposed to
provide compile-time checking. However, the only generics method on the Map
class is put(K, V). So calling get(Object) doesn't cause a compilation error,
but merely returns null.

