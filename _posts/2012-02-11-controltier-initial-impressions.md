---
layout: post
title: ! 'ControlTier: Initial Impressions'
categories: []
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

For the last 2 months, we've been using [ControlTier](http://controltier.org)
to automate deployments to our integration and QA environments.  Our
deployment is a little more complicated that most Java applications because it
runs on a pseudo-grid.  That is, there are 2 clustered JBoss servers, a
database server, and 4+ Linux servers that each run about 8 Java processes.

I was able to get ControlTier up and running in a few days, and defining the
jobs was relatively straightforward.

Features I really like include:

  1. Tagging nodes. This allows me to run, for example, the JBoss startup on all nodes tagged as "jboss".  If I add a new node I don't have to modify the job.
  2. Composite jobs. I can define small, single-purpose jobs and combine them into larger jobs. Code re-use, if you will.

There are a few features I'd really like to see:

  1. A Jenkins plug-in.  Deployments to the Integration environment always follow a successful build.
  2. Simpler parallel job configuration. I can start all Java grid processes simultaneously, but haven't figured out how to get ControlTier to do it.
  3. Ability to configure environments and run the same job against multiple environments rather than having to copy the job and change its project or target nodes.
  4. Synchronous job execution. Although this may not be necessary if there was a Jenkins plug-in.

Overall I'm happy with what [ControlTier](http://controltier.org) is doing for
us.  If I get time, I'd also like to compare it to other deployment frameworks
like [RunDeck](http://rundeck.org).

