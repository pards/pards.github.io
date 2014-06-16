---
layout: post
title: ! 'SonarException: The project is already been analysing.'
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  Description: ! 'To fix the problem: Log into the database and delete any rows in
    the ''semaphores'' table.'
  Title: ! 'SonarException: The project is already been analysing.'
  Keywords: sonarexception analysing, sonar database semaphore
  _aioseop_keywords: sonarexception analysing, sonar database semaphore
  _aioseop_description: ! 'To fix the problem: Log into the database and delete any
    rows in the ''semaphores'' table.'
  _edit_last: '2'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I was getting this error when running a Sonar analysis recently.

This was caused by the Jenkins/Sonar process getting killed while the project
analysis is underway. Sonar now uses a database semaphore to prevent multiple
builds running at the same time, but if the job gets terminated then the
semaphores don't get cleaned up.

**To fix the problem**: Log into the database and delete any rows in the 'semaphores' table.

