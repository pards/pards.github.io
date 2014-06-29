---
layout: post
title: List.clear() throws UnsupportedOperationException
categories:
- Code
tags: 
- Java
---

Consider the following code  

	String[] s = new String[]{""};  
	List<String> list = Arrays.asList(s);  
	list.clear();  

All looks good, right? When you run it you get:  

	java.lang.UnsupportedOperationException  
		at java.util.AbstractList.remove(AbstractList.java:144)  
		at java.util.AbstractList$Itr.remove(AbstractList.java:360)  
		at java.util.AbstractList.removeRange(AbstractList.java:559)  
		at java.util.AbstractList.clear(AbstractList.java:217)  

This is because Arrays.asList(array) returns a fixed-size list backed by the
specified array. Fixed-size being the operative word.
