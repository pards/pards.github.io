---
layout: post
title: JUnit categories with Maven
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_keywords: junit, maven, surefire, excludedgroups
  Title: JUnit categories with Maven
  _aioseop_description: ! 'Getting Maven''s SureFire plugin to work with your JUnit
    categories can be frustrating so here''s a few things that I learnt along the
    way. '
  _aioseop_title: JUnit categories with Maven
  Description: Getting Maven's SureFire plugin to work with your JUnit categories
    can be frustrating so here's a few things that I learnt along the way.
  Keywords: junit, maven, surefire, excludedgroups
  _syntaxhighlighter_encoded: '1'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---
-------------------------------------------------------<br />
 T E S T S<br />
-------------------------------------------------------<br />
[INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ junit-categories ---<br />
[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT<br />
[INFO] +- org.jmock:jmock-junit4:jar:2.6.0-RC2:test<br />
[INFO] |  +- org.jmock:jmock:jar:2.6.0-RC2:test<br />
[INFO] |  |  \- org.hamcrest:hamcrest-library:jar:1.3.RC2:test<br />
[INFO] |  \- junit:junit-dep:jar:4.4:test<br />
[INFO] \- junit:junit:jar:4.10:test<br />
[INFO]    \- org.hamcrest:hamcrest-core:jar:1.1:test<br />
[/plain]</p>
<p>See how jmock-junit4 depends on junit-dep:4.4, <i>and</i> that it appears <i>before</i> JUnit 4.10?  That's the cause of the error above.  You can either exclude each of these conflicting dependencies or you can make sure that the versions of JUnit and/or JUnit-dep appear first in the classpath.  The latter is achieved by putting them at the top of the dependency list in your pom.xml (or better yet, the parent pom.xml if you have one).  </p>
<p>Here's the dependency tree after moving the JUnit dependency above the jMock dependency in my pom.xml</p>
<p>[plain language="text" highlight="2,7"]<br />
[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT<br />
[INFO] +- junit:junit:jar:4.10:test<br />
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.1:test<br />
[INFO] \- org.jmock:jmock-junit4:jar:2.6.0-RC2:test<br />
[INFO]    +- org.jmock:jmock:jar:2.6.0-RC2:test<br />
[INFO]    |  \- org.hamcrest:hamcrest-library:jar:1.3.RC2:test<br />
[INFO]    \- junit:junit-dep:jar:4.4:test<br />
[/plain]</p>
<p>Tests may also fail if you have all methods annotated with a category that is excluded. In this case you need to annotate the class with the exclusion category.  This is similar to how @Ignore works.</p>

Getting Maven's SureFire plugin to work with your JUnit categories can be
frustrating so here's a few things that I learnt along the way. The first
point to note is that categories only work with surefire 2.11 or greater.

You also need to set up the surefire plugin correctly as follows:  
Create the interface that will be used to categorize your unit tests.  
[java]  
public interface SlowTest {}  
[/java]

Add the category to your test method (or class-level)  
[java]  
@Test  
@Category(SlowTest.class)  
public void excludedTestMethod() {  
Example eg = new Example();  
eg.methodTwo();  
}  
[/java]

Configure the maven-surefire-plugin. The inline dependency is very important -
Maven will ignore your @Category annotation otherwise.  
[xml]  
<plugin>  
<groupId>org.apache.maven.plugins</groupId>  
<artifactId>maven-surefire-plugin</artifactId>  
<version>2.12</version>  
<dependencies>  
<dependency>  
<groupId>org.apache.maven.surefire</groupId>  
<artifactId>surefire-junit47</artifactId>  
<version>2.12</version>  
</dependency>  
</dependencies>  
<configuration>  
<excludedGroups>com.craigpardey.test.annotation.SlowTest</excludedGroups>  
</configuration>  
</plugin>  
[/xml]

If that works for you, then you're done.

However, you may be seeing weird stacktraces in your build.

[plain language="text"]  
Concurrency config is parallel='none', perCoreThreadCount=true, threadCount=2,
useUnlimitedThreads=false  
org.apache.maven.surefire.util.SurefireReflectionException:
java.lang.reflect.InvocationTargetException; nested exception is
java.lang.reflect.InvocationTargetException: null  
java.lang.reflect.InvocationTargetException  
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)  
at
sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)  
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImp
l.java:25)  
at java.lang.reflect.Method.invoke(Method.java:597)  
at org.apache.maven.surefire.util.ReflectionUtils.invokeMethodWithArray(Reflec
tionUtils.java:189)  
at org.apache.maven.surefire.booter.ProviderFactory$ProviderProxy.invoke(Provi
derFactory.java:165)  
at org.apache.maven.surefire.booter.ProviderFactory.invokeProvider(ProviderFac
tory.java:85)  
at org.apache.maven.surefire.booter.ForkedBooter.runSuitesInProcess(ForkedBoot
er.java:103)  
at org.apache.maven.surefire.booter.ForkedBooter.main(ForkedBooter.java:74)  
Caused by: java.lang.NoSuchMethodError:
org.junit.runner.Description.getMethodName()Ljava/lang/String;  
at org.apache.maven.surefire.common.junit48.FilterFactory$GroupMatcherCategory
Filter.shouldRun(FilterFactory.java:207)  
at org.apache.maven.surefire.junitcore.JUnitCoreProvider.getSuitesAsList(JUnit
CoreProvider.java:159)  
at org.apache.maven.surefire.junitcore.JUnitCoreProvider.invoke(JUnitCoreProvi
der.java:119)

... 9 more  
Results :

Tests run: 0, Failures: 0, Errors: 0, Skipped: 0  
[/plain]

These are probably caused by having conflicting (and old) versions of JUnit in
your dependencies. To resolve these issues, start with the dependency tree

[plain language="text" highlight="7,8"]  
$ mvn dependency:tree  
[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT  
[INFO] +- org.jmock:jmock-junit4:jar:2.6.0-RC2:test  
[INFO] | +- org.jmock:jmock:jar:2.6.0-RC2:test  
[INFO] | | \\- org.hamcrest:hamcrest-library:jar:1.3.RC2:test  
[INFO] | \\- junit:junit-dep:jar:4.4:test  
[INFO] \\- junit:junit:jar:4.10:test  
[INFO] \\- org.hamcrest:hamcrest-core:jar:1.1:test  
[/plain]

See how jmock-junit4 depends on junit-dep:4.4, _and_ that it appears _before_
JUnit 4.10? That's the cause of the error above. You can either exclude each
of these conflicting dependencies or you can make sure that the versions of
JUnit and/or JUnit-dep appear first in the classpath. The latter is achieved
by putting them at the top of the dependency list in your pom.xml (or better
yet, the parent pom.xml if you have one).

Here's the dependency tree after moving the JUnit dependency above the jMock
dependency in my pom.xml

[plain language="text" highlight="2,7"]  
[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT  
[INFO] +- junit:junit:jar:4.10:test  
[INFO] | \\- org.hamcrest:hamcrest-core:jar:1.1:test  
[INFO] \\- org.jmock:jmock-junit4:jar:2.6.0-RC2:test  
[INFO] +- org.jmock:jmock:jar:2.6.0-RC2:test  
[INFO] | \\- org.hamcrest:hamcrest-library:jar:1.3.RC2:test  
[INFO] \\- junit:junit-dep:jar:4.4:test  
[/plain]

Tests may also fail if you have all methods annotated with a category that is
excluded. In this case you need to annotate the class with the exclusion
category. This is similar to how @Ignore works.

