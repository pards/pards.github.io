---
layout: post
title: How to set JMSXUserID in Websphere MQ using Spring JMS
categories: []
tags: []
status: publish
type: post
published: true
meta:
  _syntaxhighlighter_encoded: '1'
  _edit_last: '2'
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Every time I work with MQ and Spring JMS I want to poke my eyes out with a
dull instrument. It's always a pain in the neck. My most recent challenge was
trying to get the JMSXUserID field to flow through to the application on the
other side of MQ.

First, I created a MessagePostProcessor to add the JMS header  
[java]  
public class MyMessagePostProcessor implements MessagePostProcessor  
{  
private String userId;

@Override  
public Message postProcessMessage(Message msg) throws JMSException  
{  
msg.setStringProperty(JmsConstants.JMS_IBM_MQMD_USERIDENTIFIER, userId);  
msg.setStringProperty("JMSXUserID", userId);  
return msg;  
}  
}  
[/java]

But when I checked the queue with HermesJMS I could see that the JMSXUserID
had been overwritten somewhere, so I fired up WireShark and watched for the
message and confirmed that the message that was going over the wire already
had the JMSXUserID field overwritten. This was a good thing - the issue was in
the IBM MQ JMS client code.

Everywhere I looked online told me that the JMSXUserID field is set by the JMS
provider in the 'send' method, and that there was little or no possibility of
controlling it. Then I found [this reference](http://publib.boulder.ibm.com/in
focenter/wmqv7/v7r0/index.jsp?topic=%2Fcom.ibm.mq.csqzaw.doc%2Fjm41030_.htm)
on the IBM site that said:

> You must set the Destination object property WMQ_MQMD_WRITE_ENABLED to true
for the setting of MQMD properties to have any effect  
...  
It is necessary to set WMQ_MQMD_MESSAGE_CONTEXT before setting
JMS_IBM_MQMD_UserIdentifier.

However, these are properties of the Destination, to which I did not currently
have access because I was using Spring's JmsTemplate +
DynamicDestinationResolver to send the message. So I extended Spring's
DynamicDestinationResolver, set the two fields I needed to and Voila! The
JMSXUserID passed through.

[java]  
public class MqDestinationResolver extends DynamicDestinationResolver  
{  
@Override  
public Destination resolveDestinationName(Session session, String
destinationName, boolean pubSubDomain) throws JMSException  
{  
MQDestination mqDest = (MQDestination)super.resolveDestinationName(session,
destinationName, pubSubDomain);  
mqDest.setBooleanProperty(WMQConstants.WMQ_MQMD_WRITE_ENABLED, true);  
mqDest.setIntProperty(WMQConstants.WMQ_MQMD_MESSAGE_CONTEXT,
WMQConstants.WMQ_MDCTX_SET_IDENTITY_CONTEXT);  
return dest;  
}  
}  
[/java]

I hope this helps.

