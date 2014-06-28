---
layout: post
title: Converting from JUnit to TestNG
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_title: Converting from JUnit to TestNG
  _aioseop_keywords: Converting from JUnit to TestNG
  _aioseop_description: TestNG's Assert uses different argument ordering from JUnit
    which complicates the conversion process.
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

The TestNG Eclipse plugin has a feature that will convert your unit tests from
JUnit to TestNG in a few simple clicks, and it works pretty well for the most
part. However, I soon noticed that it was replacing Assert.assertX with
AssertJUnit.assertX. Upon further inspection, this was because TestNG's Assert
uses different argument ordering. For example  

    // JUnit  
    Assert.assertEquals(message, expected, actual);

    // TestNG  
    Assert.assertEquals(actual, expected, message);  

Each argument has moved.

For most people, sticking with AssertJUnit will be good enough, but we wanted
to go all-in on TestNG so I set about trying to figure out the best way to
safely refactor our tests.

First, I did a global search-and-replace in Eclipse to change AssertJUnit to
Assert. This produced a bunch of compilation errors and failing tests caused
by assertEquals(msg, string, string) that I fixed by hand. But I still had the
problem of getting incorrect failure messages like "found X, expected Y" when
it should have been "found Y, expected X".

Automating the transposition of the expected/actual arguments was the hard
part. My first instinct was to use regular expressions as explained [here on
StackOverflow](http://stackoverflow.com/questions/7718338/using-eclipse-find-
and-replace-all-to-swap-arguments) until I realised that it doesn't handle the
more complex cases like Assert.assertEquals("a", methodCall(a,b));

Then I recalled that Eclipse has a great feature to support changing the
signature of a method. Just right-click on the method name, choose "Refactor"
then "Change Method Signature..." and you'll be presented with a dialog box
that allows you to alter any aspect of the method.

But the method in question is in an API, not my code.

The trick here was to _create a local copy of the org.testng.Assert_ class in
my project. Maven and Eclipse will find my copy instead of the TestNG copy
because it appears first on the classpath. Next, I opened up my copy of
org.testng.Assert and used Eclipse's nifty refactoring tools to swap the
expected/actual arguments. This automatically (and accurately) flipped the
argument order in my test cases. Once it was done I simply deleted my local
copy of org.testng.Assert.

