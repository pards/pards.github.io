---
layout: post
title: Scheduled tasks with Spring and Java configs
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _syntaxhighlighter_encoded: '1'
  _edit_last: '2'
  _aioseop_description: Spring 3.2 has some very nice features for scheduling tasks.
  _aioseop_title: Scheduled tasks with Spring and Java configs
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Spring 3.2 has some very nice features for scheduling tasks.

The pure Java way of doing this looks something like  

    private ScheduledExecutorService executor =
    Executors.newSingleThreadScheduledExecutor();  
    class ScheduledTask implements Runnable {  
        @Override  
        public void run() {  
            System.out.println("Running scheduled task");  
        }  
    }

    // Schedule a task every 5 seconds  
    executor.scheduleAtFixedRate(new ScheduledTask(), 1, 5, TimeUnit.SECONDS);  
    // If you don't do this then the JVM won't exit cleanly  
    executor.shutdown();  

But now, with the snazzy new Spring scheduling annotations, it can be as
simple as this  

    @Configuration  
    @EnableScheduling  
    public class MyAppConfig {  
        @Bean  
        public IHeartbeatService getHeartbeatService() {  
            return new HeartbeatServiceImpl();  
        }  
    }

    public class HeartbeatServiceImpl implements IHeartbeatService {  
        private static final String HEARTBEAT_DELAY_MS = "${heartbeat.initdelay.ms}";  
        private static final String HEARTBEAT_INIT_DELAY_MS = "${heartbeat.ms}";

        @Override  
        @Scheduled(initialDelayString = HEARTBEAT_DELAY_MS, fixedDelayString = HEARTBEAT_INIT_DELAY_MS)  
        public void sendHeartbeat() {  
            LOGGER.info("Heartbeating goes here.");  
        }  
    }

The beauty of this approach is that

  * It's declarative.
  * Spring's property loading works.
  * I don't have to care about which Java executor Spring chooses under the hood.

