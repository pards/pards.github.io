---
layout: post
title: ! '@Value not resolved when using @PropertySource annotation.'
categories:
- Code
tags: 
- Java
- Spring
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

