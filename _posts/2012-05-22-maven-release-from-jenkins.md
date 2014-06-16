---
layout: post
title: Maven Release from Jenkins
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_description: Automating the Maven Release process will streamline your
    build cycle, but there are a few gotchas when using Mercurial and Windows
  _aioseop_keywords: ! 'Maven Release from Jenkins, Mercurial Maven Release, [DEBUG]
    abort: pom.xml not under root'
  Title: Maven Release from Jenkins
  Description: Automating the Maven Release process will streamline your build cycle,
    but there are a few gotchas when using Mercurial and Windows
  Keywords: ! 'Maven Release from Jenkins, Mercurial Maven Release, [DEBUG] abort:
    pom.xml not under root'
  _aioseop_title: Maven Release from Jenkins
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I encountered this error while setting up a Jenkins job to automate our Maven
release process.

`[INFO] EXECUTING: cmd.exe /X /C "hg commit --message "[maven-release-plugin]
prepare for next development iteration"
c:\jenkins\jobs\Release\workspace\pom.xml"  
[DEBUG] abort: C:\Jenkins\jobs\Release\workspace\pom.xml not under root  
[ERROR]  
EXECUTION FAILED  
Execution of cmd : commit failed with exit code: -1.  
Working directory was:  
c:\jenkins\jobs\Release\workspace  
Your Hg installation seems to be valid and complete.  
Hg version: 2.0 (OK)  
`

Our Jenkins box runs on Windows. It turns out the Mercurial (HG) is case-
sensitive in a way that hadn't impacted our project in any way up until this
point. Note the subtle differences in the first two log statements:

`c:\jenkins  
C:\Jenkins  
`

This was because our JENKINS_HOME environment variable was set to "c:\jenkins"
but HG was using "C:\Jenkins".

The solution was to change the value of the JENKINS_HOME environment variable
to the case-sensitive "C:\Jenkins", restart the service, and everything worked
fine.

