---
layout: post
title: Using Factory Beans in Spring Java Configs
categories:
- Code
tags: 
- Java
- Spring
---

I recently went through an exercise of converting Spring XML configuration
files to Java-based configuration. The process went well for the most part,
but I encountered an oddity around how to use factory beans when using Java
configs.

In XML, factory beans would be used like this  

	<bean id="MyDataSource"  
	class="org.springframework.jndi.JndiObjectFactoryBean"  
	p:jndiName="jdbc/MyDataSource"  
	p:resourceRef="false"  
	/>  

In Java however, this has to be separated into two separate @Beans  

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

