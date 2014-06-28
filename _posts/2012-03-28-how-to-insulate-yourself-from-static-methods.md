---
layout: post
title: How to insulate yourself from static methods
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_description: How to unit test static methods calls in 3rd party libraries
  _aioseop_keywords: unit test, static unit test
  Title: Unit testing static methods
  Description: How to unit test static methods calls in 3rd party libraries
  Keywords: unit test, junit, static unit test
  _aioseop_title: Unit test static methods in 3rd party libraries
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

Sometimes your code needs to call a static method in a 3rd party library and
unit testing  suddenly becomes difficult, particularly when that static method
requires context in order to work.

Enter the insulating class (also known as a Facade).

  1. Create a new interface that declares a method with the same signature as the static method you want to delegate to
  2. Create a new Facade class that implements the interface
  3. Replace the calls to the static method with calls to your new Facade class

Let's assume that the method you need to call looks like this
    
     public class ThirdPartyValueComputer {
       public static int computeValue(SomeObject param, SomeOtherObject param){
         ....
       }
     }
    

If you put this call into another class with an interface then you will be
able to use mock objects to test your code.
    
     public interface IValueComputer {
       int computeValue(SomeObject p1, SomeOtherObject p2);
     }
     
     public class ValueComputerFacade implements IValueComputer {
       @Override
       public int computeValue(SomeObject p1, SomeOtherObject p2){
         return ThirdPartyValueComputer.computeValue(SomeObject p1, SomeOtherObject p2);
       }
     }
    
     public interface ISomeService {
       int calculate(SomeObject p1, SomeOtherObject p2);
     }
     
     public class SomeService implements ISomeService {
       private IValueComputer valueComputer;
       public void setValueComputer(IValueComputer c){
         this.valueComputer = c;
       }
    
       @Override
       public int calculate(SomeObject p1, SomeOtherObject p2) {
         int computedValue = valueComputer.computeValue(p1, p2);
         // Now perform the logic you need to test
         int result = ...;
         return result;
       }
     }
    

The service is configured in Spring as follows

Now that you've created and configured your facade, you can write tests for
the service class as follows
    
     Mockery context = new Mockery();
    
     @Test
     public void testCalculate() throws Exception {
       final SomeObject p1 = new SomeObject();
       final SomeOtherObject p2 = new SomeOtherObject();
    
       IValueComputer valueComputer = context.mock(IValueComputer.class);
       SomeService service = new SomeService();
       service.setValueComputer(valueComputer);
      
       context.checking(new Expectations() {
         oneOf(valueComputer).computeValue(p1, p2))
         will(Expectations.returnValue(1));
       });
    
       int result = service.calculate(p1, p2);
       assertEquals(1, result);
     }
    

