---
layout: post
title: Error creating logs directory in Apache Tomcat
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_keywords: apache tomcat
  _aioseop_title: Error creating logs directory in Apache Tomcat
  Title: Error creating logs directory in Apache Tomcat
  Keywords: apache tomcat
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

`java.util.logging.ErrorManager: 4: Unable to create [c:\apache-tomcat-7.0.30"
-Dcatalina.home=c:\apache-tomcat-7.0.30"\logs\]  
`

I was getting this error recently, and found that it was caused by having a
trailing backslash on my CATALINA_HOME variable.

i.e.  
Incorrect:  
`set CATALINA_HOME=c:\apache-tomcat-7.0.30\`

Correct:  
`set CATALINA_HOME=c:\apache-tomcat-7.0.30`

