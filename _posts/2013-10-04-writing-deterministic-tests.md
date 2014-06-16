---
layout: post
title: Writing deterministic tests
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_description: Non-deterministic unit tests are like kryptonite for an automated
    build. They cause intermittent build failures and the development team start to
    ignore the failing build. This is a Bad Thing.
  _aioseop_keywords: unit test, junit, thread.sleep, deterministic testing, multi-threaded
    testing, junit multi-threaded
  _aioseop_title: Writing deterministic tests
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Non-deterministic unit tests are like kryptonite for an automated build. They
cause intermittent build failures and the development team becomes accustomed
to ignoring a failing build. This is a Bad Thing.

Thread.sleep(x) in a unit test is a recipe for non-deterministic behaviour.
Think about what happens when this test is run on a slow machine. How big does
X need to be?

I often see this anti-pattern in multi-threaded code where the test case is
waiting for some asynchronous task to complete, and the test case has to guess
at how long the task will take, and then asserts on some condition that the
task was supposed to change.

The other factor to consider is that Thread.sleep(x) will always wait X
milliseconds even if the task finished earlier than you expected. The more of
these you add, the slower your test suite will be.

You need to use deterministic waits.

By that, I mean using a callback method plus wait()/notify().

Consider the following example:

[java]  
@Test  
public void testSomeAsynchCode() throws Exception  
{  
AsynchObj asynchObj = new AsynchObj();  
final Object lock = new Object();  
asynchObj.addListener(new AsynchListener()  
{  
@Override  
public void asynchCodeDone()  
{  
synchronized (lock)  
{  
System.out.println("Listener notified");  
lock.notify();  
}  
}  
});

synchronized (lock)  
{  
System.out.println("Running asynch code");  
asynchObj.asynchCode();  
lock.wait(TIMEOUT);  
System.out.println("Test notified");  
}  
assertEquals(2, asynchObj.getSomeValue());  
}  
[/java]

[java]  
public interface AsynchListener  
{  
void asynchCodeDone();  
}  
[/java]

[java]  
public class AsynchObj  
{  
private final int MAX_WAIT = 4000;  
private int someValue;  
private Set<AsynchListener> listeners = new HashSet<AsynchListener>();  
private ExecutorService executor = Executors.newCachedThreadPool();

public void addListener(AsynchListener listener)  
{  
listeners.add(listener);  
}

public void asynchCode()  
{  
executor.submit(new AsynchObjTask());  
}

class AsynchObjTask implements Runnable  
{  
private Random r = new Random();  
public void run()  
{  
randomWait();  
setSomeValue(2);  
for (AsynchListener listener : listeners)  
{  
listener.asynchCodeDone();  
}  
}

private void randomWait()  
{  
int wait = r.nextInt(MAX_WAIT);  
try  
{  
System.out.println(String.format("Waiting for %d ms", wait));  
Thread.sleep(wait);  
}  
catch (InterruptedException e)  
{  
}  
}  
}

public void setSomeValue(int x)  
{  
this.someValue = x;  
}

public int getSomeValue()  
{  
return this.someValue;  
}  
}  
[/java]

Here's the full [source code](http://craigpardey.com/wp/wp-
content/uploads/2013/10/asynch.zip).

In this example, AsynchObj will notify the test as soon as it has run the
asynchronous code. The test case will immediately run the assertion.

This will run exactly the same way on a slow machine as it will on a fast
machine. It will pass (or fail) consistently.

Say goodbye to intermittent build failures!

