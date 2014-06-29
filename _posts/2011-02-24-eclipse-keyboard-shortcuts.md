---
layout: post
title: Eclipse keyboard shortcuts
categories:
- IDE
tags: 
- Eclipse
---

When I first started working at Intelliware I was amazed at how efficient the
developers were while working in Eclipse. Most of this efficiency and speed
came from a deep knowledge of Eclipse's keyboard shortcuts.

Here is my list of Eclipse keyboard shortcuts that every developer should
know.

  1. **Ctrl-Shift-L**: Show list of shortcuts
  2. **Ctrl-K**: Find next
  3. **Ctrl-Shift-R**: Open a file in your workspace. It supports CamelCase shorthand too, so "SBFil" will find "SomeBigFile.java" and "SBFile.java"
  4. **Ctrl-Shift-G**: Find references for a class or method. Want to find who calls MyClass.getName()? Put your cursor on the getName() definition and hit Ctrl-Shift-G
  5. **F3**: Drill into a method. This will jump from the code that calls the method into the method definition. Similar to holding Ctrl and clicking on the method call.
  6. **Ctrl-T**: Open implementors of an interface. Very useful when using F3 to follow the flow of method calls.
  7. **Ctrl-O**: Open a list of method declarations in the current class. Allows you to jump directly to the method using the arrow keys and enter. **Bonus points**: typing Ctrl-O again while the list is showing will also list method declarations from all superclasses
  8. **Ctrl-E**: Shows a list that allows you to switch between open files
  9. **Alt-Shift-W**: Locates the current file in the Navigation or Package Explorer tree.
  10. **Ctrl-L**: Go to line number. You might want to show line numbers by default by changing the preference General>Editors>Text Editors
  11. **Ctrl-H**: Find a string in all files. You can customize this to only show the File search by clicking the Customize button and unchecking the other options
  12. **Ctrl-Shift-O**: Organize import statements. This also removes any unused imports.
  13. **Ctrl-Shift-F**: Reformat the file

You should also familiarize yourself with the following debugging shortcuts

  1. **F5**: Step into
  2. **F6**: Step over
  3. **F7**: Step back out
  4. **F8**: Resume (i.e. continue to next breakpoint)
  5. **Ctrl-Shift-I**: Show current value of variable
  6. **Ctrl-R**: Run to current line

