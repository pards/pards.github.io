---
layout: post
title: Use affirmative phrases for boolean names
categories:
- Code
tags: 
- Variable naming
---

Please use affirmative phrases when naming your boolean variables and flags -
I find that it makes code much more readable.  This is particularly true when
naming the configuration settings used to turn features on or off.  If the
setting is named in the negative then I have to use a double negative to
enable it, and everyone learnt in high school that double negatives are a Bad
Thing.

For example, if I have a feature that sends emails and I want to make this
behaviour conditional then an appropriate variable name would be "boolean
sendEmail = true".  It's easily comprehended.  Contrast that with the same
feature phrased negatively: "boolean disableEmail = false".

It gets even more confusing when these variables get used in the code

    
    
    if( sendEmail) {
      sendEmail();
    }

as opposed to

    
    
    if( !disableEmail) {
      sendEmail();
    }

For bonus points, be explicit about the default value especially if the value
is coming from a configuration file where it might be absent entirely.

