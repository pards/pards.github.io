---
layout: post
title: Arrays.asList() gives a list with wrong size
categories:
- Code
tags: 
- Java
---

Try this out for fun.

	int[] i = new int[]{1, 2, 3};  
	System.out.println(Arrays.asList(i).size());

	Integer[] ii = new Integer[]{1, 2, 3};  
	System.out.println(Arrays.asList(ii).size());  

Output:  
	1  
	3

What the hell, Java? This is totally unexpected behaviour!

In this case Java is being a bit pedantic. See, Collections can contain only
contain Objects, and an `int` is not an `Object` but `int[]` is an `Object`,
so that's why you get a list with only 1 element.

But really, the language should protect against these kinds of needle-
in-a-haystack bugs.

