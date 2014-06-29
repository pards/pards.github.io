---
layout: post
title: JUnit categories with Maven
categories:
- Testing
tags: 
- JUnit
---

	-------------------------------------------------------
	 T E S T S
	-------------------------------------------------------
	[INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ junit-categories ---
	[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT
	[INFO] +- org.jmock:jmock-junit4:jar:2.6.0-RC2:test
	[INFO] |  +- org.jmock:jmock:jar:2.6.0-RC2:test
	[INFO] |  |  \- org.hamcrest:hamcrest-library:jar:1.3.RC2:test
	[INFO] |  \- junit:junit-dep:jar:4.4:test
	[INFO] \- junit:junit:jar:4.10:test
	[INFO]    \- org.hamcrest:hamcrest-core:jar:1.1:test

See how jmock-junit4 depends on junit-dep:4.4, <i>and</i> that it appears <i>before</i> JUnit 4.10?  That's the cause of the error above.  You can either exclude each of these conflicting dependencies or you can make sure that the versions of JUnit and/or JUnit-dep appear first in the classpath.  The latter is achieved by putting them at the top of the dependency list in your pom.xml (or better yet, the parent pom.xml if you have one).  
Here's the dependency tree after moving the JUnit dependency above the jMock dependency in my pom.xml

	[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT
	[INFO] +- junit:junit:jar:4.10:test
	[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.1:test
	[INFO] \- org.jmock:jmock-junit4:jar:2.6.0-RC2:test
	[INFO]    +- org.jmock:jmock:jar:2.6.0-RC2:test
	[INFO]    |  \- org.hamcrest:hamcrest-library:jar:1.3.RC2:test
	[INFO]    \- junit:junit-dep:jar:4.4:test

Tests may also fail if you have all methods annotated with a category that is excluded. In this case you need to annotate the class with the exclusion category.  This is similar to how @Ignore works.

Getting Maven's SureFire plugin to work with your JUnit categories can be
frustrating so here's a few things that I learnt along the way. The first
point to note is that categories only work with surefire 2.11 or greater.

You also need to set up the surefire plugin correctly as follows:  
Create the interface that will be used to categorize your unit tests.  

	public interface SlowTest {}  

Add the category to your test method (or class-level)  

	@Test  
	@Category(SlowTest.class)  
	public void excludedTestMethod() {  
		Example eg = new Example();  
		eg.methodTwo();  
	}  

Configure the maven-surefire-plugin. The inline dependency is very important -
Maven will ignore your @Category annotation otherwise.  

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

If that works for you, then you're done.

However, you may be seeing weird stacktraces in your build.

	Concurrency config is parallel='none', perCoreThreadCount=true, threadCount=2,useUnlimitedThreads=false  
	org.apache.maven.surefire.util.SurefireReflectionException:
	java.lang.reflect.InvocationTargetException; nested exception is
	java.lang.reflect.InvocationTargetException: null  
	java.lang.reflect.InvocationTargetException  
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)  
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)  
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)  
	at java.lang.reflect.Method.invoke(Method.java:597)  
	at org.apache.maven.surefire.util.ReflectionUtils.invokeMethodWithArray(ReflectionUtils.java:189)  
	at org.apache.maven.surefire.booter.ProviderFactory$ProviderProxy.invoke(ProviderFactory.java:165)  
	at org.apache.maven.surefire.booter.ProviderFactory.invokeProvider(ProviderFactory.java:85)  
	at org.apache.maven.surefire.booter.ForkedBooter.runSuitesInProcess(ForkedBooter.java:103)  
	at org.apache.maven.surefire.booter.ForkedBooter.main(ForkedBooter.java:74)  
	Caused by: java.lang.NoSuchMethodError:
	org.junit.runner.Description.getMethodName()Ljava/lang/String;  
	at org.apache.maven.surefire.common.junit48.FilterFactory$GroupMatcherCategoryFilter.shouldRun(FilterFactory.java:207)  
	at org.apache.maven.surefire.junitcore.JUnitCoreProvider.getSuitesAsList(JUnitCoreProvider.java:159)  
	at org.apache.maven.surefire.junitcore.JUnitCoreProvider.invoke(JUnitCoreProvider.java:119)
	... 9 more  
	Results :

	Tests run: 0, Failures: 0, Errors: 0, Skipped: 0  

These are probably caused by having conflicting (and old) versions of JUnit in
your dependencies. To resolve these issues, start with the dependency tree

	$ mvn dependency:tree  
	[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT  
	[INFO] +- org.jmock:jmock-junit4:jar:2.6.0-RC2:test  
	[INFO] | +- org.jmock:jmock:jar:2.6.0-RC2:test  
	[INFO] | | \\- org.hamcrest:hamcrest-library:jar:1.3.RC2:test  
	[INFO] | \\- junit:junit-dep:jar:4.4:test  
	[INFO] \\- junit:junit:jar:4.10:test  
	[INFO] \\- org.hamcrest:hamcrest-core:jar:1.1:test  

See how jmock-junit4 depends on junit-dep:4.4, _and_ that it appears _before_
JUnit 4.10? That's the cause of the error above. You can either exclude each
of these conflicting dependencies or you can make sure that the versions of
JUnit and/or JUnit-dep appear first in the classpath. The latter is achieved
by putting them at the top of the dependency list in your pom.xml (or better
yet, the parent pom.xml if you have one).

Here's the dependency tree after moving the JUnit dependency above the jMock
dependency in my pom.xml

	[INFO] com.craigpardey:junit-categories:jar:1.0-SNAPSHOT  
	[INFO] +- junit:junit:jar:4.10:test  
	[INFO] | \\- org.hamcrest:hamcrest-core:jar:1.1:test  
	[INFO] \\- org.jmock:jmock-junit4:jar:2.6.0-RC2:test  
	[INFO] +- org.jmock:jmock:jar:2.6.0-RC2:test  
	[INFO] | \\- org.hamcrest:hamcrest-library:jar:1.3.RC2:test  
	[INFO] \\- junit:junit-dep:jar:4.4:test  

Tests may also fail if you have all methods annotated with a category that is
excluded. In this case you need to annotate the class with the exclusion
category. This is similar to how @Ignore works.
