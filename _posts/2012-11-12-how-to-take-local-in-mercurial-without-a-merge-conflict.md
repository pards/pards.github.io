---
layout: post
title: How to take local in Mercurial without a merge conflict
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  Title: How to take local in Mercurial without a merge conflict
  _aioseop_title: How to take local in Mercurial without a merge conflict
  _aioseop_keywords: mercurial, merge, take local, take other
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

When there's a merge conflict in Mercurial, TortoiseHG will give you the
option to 'take local' or 'take other' in addition to resolving the conflict.

However, sometimes you may want to 'take local' or 'take other' when there's
_no_ merge conflict.

To take local:  

	hg merge --tool internal:local <rev>  

To take other:  
	
	hg merge --tool internal:other <rev>  


