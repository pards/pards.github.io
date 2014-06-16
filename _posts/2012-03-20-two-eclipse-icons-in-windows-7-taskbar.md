---
layout: post
title: Two Eclipse Icons in Windows 7 taskbar
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_title: Two Eclipse Icons in Windows 7 taskbar
  _aioseop_description: Seeing two Eclipse Icons in Windows 7 taskbar? Here's how
    to fix it.
  Title: Two Eclipse Icons in Windows 7 taskbar
  Description: Seeing two Eclipse Icons in Windows 7 taskbar? Here's how to fix it.
  Keywords: eclipse, windows 7
  _aioseop_keywords: eclipse, windows 7
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I had an annoying problem on Windows 7 x64 with Helios x64 where I had pinned
Eclipse to the taskbar but would see two icons whenever Eclipse was running. I
found that the following workaround works with the option "Always combine,
hide labels" for taskbar buttons.

  * You must specify a -vm argument in your "eclipse.ini" file (in your Eclipse directory).
  * The -vm argument in your "eclipse.ini" must point to the **bin** directory of your JDK or JRE (and **_not to javaw.exe_**). For me the argument is "C:/Program Files/Java/jdk1.6.0_27/bin/" without quotes.
  * Unpin Eclipse from the taskbar or delete the shortcut
  * Run "eclipse.exe" from Windows Explorer and choose your workspace
  * Pin Eclipse to the taskbar after the splash screen has loaded and the main window is shown

Hope this helps.

