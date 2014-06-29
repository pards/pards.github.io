---
layout: post
title: Mercurial changes how you work
categories:
- Version Control
tags: [Mercurial]
---

I've recently started using [Mercurial](http://mercurial.selenic.com/), which
is a [distributed version control
system](http://en.wikipedia.org/wiki/Distributed_revision_control).

DVCSs initially struck me as a solution looking for a problem. SubVersion
seemed good enough for our purposes at work, but we decided to try out
Mercurial for one iteration to see what all the fuss was about.

Perhaps the most significant difference between SubVersion and Mercurial is
that Mercurial uses a local repository. This subtle distinction significantly
changes how you work because you can commit your changes without having to
publish them. A 'commit' in Mercurial is like a savepoint in database-speak -
it gives you a safe place to roll back to if things go awry. Once you're happy
with your changes you can use the patch queue to fold all your commits into a
single changeset and 'push' (i.e. publish) your changes to the central
repository.

With Mercurial I commit early and commit often.

