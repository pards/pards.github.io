---
layout: post
title: Continuous deployment
categories:
- DevOps
tags: 
- Continuous Deployment
---

Automation isn't about quality, it's about scale.  Manual processes don't
scale.

Each part of your process that you automate gets you one step closer to
continuous deployment. For many teams, automation is a Quadrant 4 activity -
not urgent, not important - but on my teams it is a Quadrant 2 activity -
important but not urgent. We allocate time each iteration to improve our
automation because it frees up more time for development.

Things we automate include

  * Build (Jenkins)
  * Testing (JUnit, Selenium)
  * Deployment (ControlTier)

On a typical project the deployment is the last part to be automated, however
on my current project we've strived to automate our deployment since Day One
simply because we're deploying to a grid with many moving parts.  Automation
allows us to scale our deployment without taking time away from our
development activities.

Automated processes also serve as executable documentation.  The precise steps
to perform a function are documented in code and are repeatable.  The only way
the process can change is if the automation that controls it also changes, so
the documentation is always up-to-date. Computers don't forget to do things
the way that IT Operations staff do.

Our deployment works. Repeatedly. Because it's automated.

