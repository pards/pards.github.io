---
layout: post
title: hg clone fails with 255 error
categories:
- Version Control
tags: 
- Mercurial
---

I recently encountered an issue with cloning a Mercurial repository where it
would fail with an obtuse error stating that "`[command returned code 255 Mon
Mar 25 11:39:55 2013]`". My initial Googling implied that this was caused by a
server timeout but the problem persisted even after increasing the timeout on
our Apache web server.

The clone was always failing on one particular file - a 600Mb binary. (Don't
ask...)

To get around the problem, I zipped up an existing clone of the repository and
copied it to the target machine. The developer then tried to switch to the
branch he needed to be on.

	hg update SomeBranchName  
	abort: out of memory  

Close, but no cigar. But maybe available memory on the workstation is the
culprit here and not some obscure timeout setting on the web server. There's a
page on the Mercurial wiki that discusses how [large
files](http://mercurial.selenic.com/wiki/HandlingLargeFiles) are handled.
Poorly, as it turns out.

On my workstation, I repeated the steps the developer was taking and monitored
the memory usage. When it hit the offending 600Mb file the memory usage jumped
to 1.2Gb for a split-second and then dropped back to 600Mb while it copied the
file.

The developer's workstation only has 3Gb of RAM so he closed down all other
running programs and tried to update again but it _still did not work_!

You can imagine how frustrating this was becoming, and by this point I've
already lost the developer as a potential Mercurial convert.

As a last ditch effort, I removed the large file from the branch he was
interested in, pushed the change and asked him to pull, then update to the
appropriate branch. It worked, but it was far from ideal.

The moral of this <del>story</del> rant is that Mercurial is not very good at
handling large files, particularly on an underpowered workstation. Even worse,
there's no way easy way to remove a problematic file after it has been checked
in - it'll always be in the history.

