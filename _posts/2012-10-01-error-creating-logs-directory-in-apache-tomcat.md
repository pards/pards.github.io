---
layout: post
title: Error creating logs directory in Apache Tomcat
categories:
- Web Development
tags: 
- Tomcat
---

	java.util.logging.ErrorManager: 4: Unable to create [c:\apache-tomcat-7.0.30 -Dcatalina.home=c:\apache-tomcat-7.0.30\logs\]

I was getting this error recently, and found that it was caused by having a trailing backslash on my CATALINA_HOME variable.

i.e.  
Incorrect:  

	set CATALINA_HOME=c:\apache-tomcat-7.0.30\

Correct:  

	set CATALINA_HOME=c:\apache-tomcat-7.0.30

