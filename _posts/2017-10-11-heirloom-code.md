---
layout: post
title: "Heirloom Code"
description: "Heirloom Code"
category: programming
tags: [programming, development]
---

[Michael Feathers](https://www.amazon.ca/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052) says that legacy code is any code without tests but I'd go a step further and say that legacy code is any code that is still running even though the original developer(s) are no longer working on it.  And there is certainly plenty of _that_ in financial institutions.  There are other characteristics too, such as:

- unreproducible production environment
- incomplete source control

-- missing files
-- 'master' fails to build

- no automated build process
- no release process
- few or no unit tests
- no way to run or test the code on a developer workstation
- little or no documentation
- none of the original developers available

I am presently working on a codebase that checks _all of the above_ boxes and it isn't even that old but instead of dismissing it as an unmaintainable mess I have changed my mindset and have started referring to it as Heirloom Code.  

A [legacy](https://www.merriam-webster.com/dictionary/legacy) is something received from a predecessor with no implied value - it is simply left behind - whereas an [heirloom](https://www.merriam-webster.com/dictionary/heirloom) is something _of special value_ handed down from one generation to another.  Granted it's a bit dramatic but often old code fits this description - it is still running, it is critical to the operation of the company, and it's been handed down from the previous cohort of developers.

Heirlooms are treasured and are treated with care and respect even if they only serve to remind us of how things used to be.

So I have approached my newly inherited heirloom code as a restoration project and I'm trying to restore it to its former glory.  The first step was getting a reproducible production deployment so I spent a few weeks getting everything into source control, creating an automated build that publishes to an artifact repository and then added a few simple release scripts which was then released to production - there were _no functional changes_ in this release; its sole purpose was to make certain that production was reproducible.

On the process front, the support staff are no longer permitted to just patch files in the production environment - the only way to production is through source control - which ensures that the production environment remains reproducible.  The new semi-automated release process makes this less painful and less scary so there wasn't much resistance.  "Make it easy to do The Right Thing", right? 

Next up, I slowly made minor changes so that I could run the code on my workstation, refactored a few critical classes to make them testable, added some unit tests, and released that.  In parallel, I built up a base of documentation describing how things worked mostly via a few crude system diagrams.

The codebase is now in a maintainable state and all it took was a little care and feeding.  We're releasing new functionality regularly and this critical component has a new lease on life.  It's a long way from perfect and still runs on outdated technology but at least it's less of a liability than it was.