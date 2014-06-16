---
layout: post
title: Using Factory Beans in Spring Java Configs
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_keywords: Spring Factory Bean, Spring factory bean Java Configs, spring
    java config
  _aioseop_title: Using Factory Beans in Spring Java Configs
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I recently went through an exercise of converting Spring XML configuration
files to Java-based configuration. The process went well for the most part,
but I encountered an oddity around how to use factory beans when using Java
configs.

In XML, factory beans would be used like this  
[xml]  
<bean id="MyDataSource"  
class="org.springframework.jndi.JndiObjectFactoryBean"  
p:jndiName="jdbc/MyDataSource"  
p:resourceRef="false"  
/>  
[/xml]

In Java however, this has to be separated into two separate @Beans  
[java]  
@Bean  
public JndiObjectFactoryBean getJndiObjectFactoryBean() {  
JndiObjectFactoryBean jndi = new JndiObjectFactoryBean();  
jndi.setJndiName("jdbc/MyDataSource");  
jndi.setResourceRef(false);  
return jndi;  
}

@Bean  
public DataSource getSampleDataSource() {  
return (DataSource) getJndiObjectFactoryBean().getObject();  
}  
[/java]

