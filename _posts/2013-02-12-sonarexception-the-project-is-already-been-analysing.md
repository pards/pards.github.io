---
layout: post
title: ! 'SonarException: The project is already been analysing.'
categories:
- Tools
tags: 
- Sonar
---

I was getting this error when running a Sonar analysis recently.

This was caused by the Jenkins/Sonar process getting killed while the project
analysis is underway. Sonar now uses a database semaphore to prevent multiple
builds running at the same time, but if the job gets terminated then the
semaphores don't get cleaned up.

**To fix the problem**: Log into the database and delete any rows in the 'semaphores' table.

