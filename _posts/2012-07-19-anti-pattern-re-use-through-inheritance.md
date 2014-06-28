---
layout: post
title: ! 'Anti-pattern: Re-use through inheritance'
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _syntaxhighlighter_encoded: '1'
  _aioseop_title: ! 'Anti-pattern: Re-use through inheritance '
  _aioseop_description: ! '"Re-use through inheritance" is an anti-pattern that produces
    systems that are difficult to test.'
  _aioseop_keywords: Anti-pattern, Re-use through inheritance , object composition
  Title: ! 'Anti-pattern: Re-use through inheritance '
  Description: ! '"Re-use through inheritance" is an anti-pattern that produces systems
    that are difficult to test.'
  Keywords: Anti-pattern, Re-use through inheritance , object composition
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

When I first started programming in object oriented languages I used
inheritance to reduce code duplication. You know the deal - you have two
classes that do the same sort of thing so you create an abstract superclass
and extract the common code into methods in the superclass. Voilà. No more
duplicated code.

But I was unwittingly creating an untestable codebase.

I have worked extensively with dependency injection containers over the last
five years and have begun to rely on the clean code they facilitate. DI
encourages loosely coupled classes and interface abstractions, so code re-use
is typically achieved by extracting the common code into its own class and
injecting that class as a collaborator.

The beauty of this approach is that it makes it really easy to write unit
tests for the common code. You don't have to construct the subclass with all
of its context; you can test the most important parts of the system in
isolation. Better yet, the aforementioned subclasses can be tested using Mock
Objects in place of the collaborator.

	public abstract class AbstractHandler {  
		protected final void process(Event e){  
			// Do common stuff  
		}  
	}

	public class MyHandler extends AbstractHandler {  
		public void handleEvent(Event e) {  
			// Do a bunch of stuff  
			process(e);  
			// Do more stuff  
		}  
	}  

In the contrived example above, it is impossible to test MyHandler without
also exercising AbstractHandler. In a real-world case, AbstractHandler may
have a whole bunch of external dependencies that would make it difficult to
instantiate in a test case.

So, let's refactor it a little.

	public interface IEventProcessor {  
		void process(Event e);  
	}

	public class EventProcessor implements IEventProcessor {  
		@Override  
		public void process(Event e){  
			// Do common stuff  
		}  
	}

	public class MyHandler {  
		private IEventProcessor eventProcessor;

		public void handleEvent(Event e) {  
			// Do a bunch of stuff  
			eventProcessor.process(e);  
			// Do more stuff  
		}

		void setEventProcessor(IEventProcessor eventProcessor) {  
			this.eventProcessor = eventProcessor;  
		}  
	}  

After refactoring to favour Object Composition we can easily write unit tests
for both classes. For example, to test MyHandler we use a mock IEventProcessor
(using Mockito, or JMock) so the test is not dependent on the concrete
EventProcessor class.

	public class MyHandlerTest {  
		MyHandler handler;  
		IEventProcessor processor;

		@Before  
		public void setUp() {  
			processor = mock(IEventProcessor.class);  
			handler = new MyHandler();  
			handler.setEventProcessor(processor);  
		}

		@Test  
		public void testHandleEvent() {  
			handler.handleEvent(new Event());  
		}  
	}  

Teasing apart an older, inheritance-based system into reusable parts is an
immensely valuable activity even if you're not planning to use dependency-
injection. Your system will be comprised of cleaner, independent classes, and
be more testable to boot.

	