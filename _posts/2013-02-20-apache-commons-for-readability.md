---
layout: post
title: Apache commons for readability
categories:
- Software Development
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

There are many short methods in the [Apache
Commons](http://commons.apache.org/lang/api-release/index.html) libraries that
seem like overkill at first glance, however, their purpose becomes more
apparent when you examine the effect that these methods have on the
readability of the caller.

I see a lot of code that does stuff like this:  

	if( s == null || "".equals(s.trim())) {  
		// Do something  
	}  

which could be re-written as  

	if( StringUtils.isBlank(s)) {  
		// Do something  
	}  

Personally, I find it much easier to understand the intent of the second
version.

Same goes for [the admittedly poorly named] 'defaultString'  

	String result = s1;  
	if( s1 == null) {  
		result = s2;  
	}  

which could be re-written as  

	String result = StringUtils.defaultString(s1, s2);  

There's no need to re-invent the wheel. Many smart people have worked hard to
write these methods in an efficient, bug-free way so you might as well
leverage all their hard work.

By using the tried-and-true Apache Commons classes where possible your code
will be more readable and its intended behaviour will be clearer.