---
layout: post
title: Atomic construction
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_keywords: Atomic construction, java multi-threading, double-checked locking,
    synchronization
  _aioseop_title: Atomic construction
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Bugs caused by multi-threading can be very difficult to find. Here's one I
encountered recently.

	private static MyObject INSTANCE;

	public void init() {  
		if( INSTANCE == null) {  
			initInstance();  
		}  
	}

	private static synchronized void initInstance() {  
		if( INSTANCE != null) {  
			return;  
		}  
		INSTANCE = new MyObject();  
		INSTANCE.initThis();  
		INSTANCE.initThat();  
	}  

At first glance, this code is fine. It uses a synchronized static method to
create the singleton, using double-checked locking to make sure it only gets
created once. But on closer inspection it is possible for the null check in
init() to pass before the INSTANCE has been fully initialized.

My instinct was to fix this using a lock but then a colleague pointed out that
it could be resolved locklessly by using a temporary variable instead. The
singleton only becomes non-null once the multi-step initialization has
completed.

The working, thread-safe version looks like this:  

	private static INSTANCE;

	public void init() {  
		if( INSTANCE == null) {  
			initInstance();  
		}  
	}

	private static synchronized void initInstance() {  
		if( INSTANCE != null) {  
			return;  
		}  
		MyObject tmp = new MyObject();  
		tmp.initThis();  
		tmp.initThat();  
		INSTANCE = tmp;  
	}  

