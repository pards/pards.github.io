---
layout: post
title: ! 'Tapestry: UpdateComponents, Eventlistener and Script files'
categories:
- Web Development
tags: [Tapestry]
---

This week I encountered an interesting issue in Tapestry when I tried to
dynamically load a component using Tapestry's built-in EventListener
functionality. The component in question had a `.script` file associated with
it, which Tapestry loaded dynamically, but the JavaScript functions in the
`.script` file were "not found" when I tried to execute them.

After a bit of digging around, a colleague of mine noticed that Tapestry was
loading the `.script` file using an `eval()` statement instead of inserting
the script into the DOM.

In order for the JavaScript functions to be usable we had to change the script
file to save anonymous functions into variable names.

Before:  

	function someFunctionName() {  
	...;  
	}

After:  

	someFunctionName = function() {  
	...;  
	}`

