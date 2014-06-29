---
layout: post
title: Broken Generics
categories:
- Code
tags: 
- Java
---

Given the following code  

	Map<String, String> map = new HashMap<String, String>();  
	map.put("123", "value");

	Integer key = Integer.valueOf(123);  
	String actual = map.get(key);  

What is the value of "actual"?

  1. Compilation error - key should be a String
  2. "value"
  3. null

Answer: null

This blew me away. The whole point of type-safe collections was supposed to
provide compile-time checking. However, the only generics method on the Map
class is put(K, V). So calling get(Object) doesn't cause a compilation error,
but merely returns null.

