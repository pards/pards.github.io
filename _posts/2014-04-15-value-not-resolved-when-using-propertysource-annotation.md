---
layout: post
title: ! '@Value not resolved when using @PropertySource annotation.'
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_title: ! '@Value not resolved when using @PropertySource annotation.'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Spring's gotcha-of-the-day is around using @Value to resolve property
placeholders in combination with @PropertySource.

I had the following Spring Java configuration:  

    @Configuration  
    @PropertySource("classpath:/test.properties")  
    public class TestAppConfig {

        @Value("${queue.name}")  
        private String queue;
        
        @Bean(name="queue")  
        public String getQueue() {  
            return queue;  
        }  
    }  

But the value in "queue" was not resolving - it returned "${queue.name}" as
the value.

It turns out that I needed the following magical incantation to get it to
work.  

    @Bean  
    public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {  
        return new PropertySourcesPlaceholderConfigurer();  
    }  

