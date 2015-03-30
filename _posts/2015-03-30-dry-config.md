---
layout: post
title: "DRY Configuration"
description: ""
category: spring
tags: []
---

Externalizing application configuration values into a properties file is a common pattern in enterprise applications, because it gets the environment-specific settings out of the code and avoids the need for environment-specific builds. Being able to deploy the same binary to multiple environments (Dev, QA, UAT, Production) is a Good Thing.

I came across an interesting quirk in an application recently. I needed to change the port of the application server for one of the app’s components, and the port value was wired up via Spring using some code that looked like this:

    @Value("${app.port}")
    private String appPort;

So I started the Java virtual machine with `-Dapp.port=9090` thinking that this would be sufficient, but the application did not behave as expected. I opened up the properties file and found this:

    app.host=localhost
    app.port=8080
    app.url=http://localhost:8080/

On the surface this looks fine until you consider the case above because now we have app.port=9090, but the app.url is still using http://localhost:8080/. In order to change the application port I would actually need to start the JVM with -Dapp.port=9090 -Dapp.url=http://localhost:9090/.

This is a subtle case of duplication, and one of the tenets of good software development is DRY (Don’t Repeat Yourself), so instead of specifying two parameters I took advantage of Spring’s property substitution and modified the property file to remove the duplication.

    app.host=localhost
    app.port=8080
    app.url=http://${app.host}:${app.port}/

Now I can change the port by overriding a single environment variable: `-Dapp.port=9090`

