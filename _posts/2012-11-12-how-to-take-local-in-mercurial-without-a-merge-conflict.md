---
layout: post
title: How to take local in Mercurial without a merge conflict
categories:
- Version Control
tags:
- Mercurial
---

When there's a merge conflict in Mercurial, TortoiseHG will give you the
option to 'take local' or 'take other' in addition to resolving the conflict.

However, sometimes you may want to 'take local' or 'take other' when there's
_no_ merge conflict.

To take local:  

	hg merge --tool internal:local <rev>  

To take other:  
	
	hg merge --tool internal:other <rev>  


