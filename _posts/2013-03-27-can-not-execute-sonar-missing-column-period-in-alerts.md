---
layout: post
title: ! 'Can not execute Sonar: Missing column: period in ALERTS'
categories: 
- Tools
tags: 
- Sonar
---

I recently encountered the following error in Sonar 3.5  

	[ERROR] Failed to execute goal org.codehaus.mojo:sonar-maven-plugin:2.0:sonar
	(default-cli) on project smartfx: Can not execute Sonar: Missing column:period in DB.ALERTS -> [Help 1]  
	org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal
	org.codehaus.mojo:sonar-maven-plugin:2.0:sonar (default-cli) on project	myproject: 
	Can not execute Sonar  
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:	203)  
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:	148)  
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:	140)  
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:84)  

A quick check of the database revealed that the ALERTS table does indeed
contain a column named 'period'.

**Solution**: Maven was pointing to the wrong database (sonar.jdbc.url in conf/settings.xml).

