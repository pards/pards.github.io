---
layout: post
title: How to divide without using division operator
categories:
- Code
tags: 
- Java
- Challenge
---

I attended a talk at the Toronto Agile Tour on Deliberate Practice that
focused on TDD Games and other programming challenges like Kata.  Among the
challenges were things like solving a problem without using the 'if' keyword,
or without using primitives.

Solving simple problems using these challenges makes us better programmers
because they force us to come up with multiple, creative solutions to the same
problem.

Here's my solution for how to divide without using the division operator ("/")
in Java. The method should take a numerator and a denominator and return the
largest integer multiplier. For example 10/3 = 3, which would be called as int
result = divide(10, 3)

    public interface DivisionOperator {
      int divide(int numerator, int denominator);
    }
    
    /** ************************************* */
    public class Division implements DivisionOperator {
      public int divide(int numerator, int denominator) {
        if( denominator == 0){
          throw new IllegalArgumentException("Denonimnator cannot be zero");
        }
        int i = 1;
        while( denominator * i <= numerator){
          i++;
        }
        return i - 1;
      }
    }
    
    /** ************************************* */
    public class DivisionTest {
      @Test
      public void testDivision() {
        checkDivisionOperatorImplementation(new Division());
        checkDivisionOperatorImplementation(new DivisionAlternateAnswer());
      }
    
      private void checkDivisionOperatorImplementation( DivisionOperator div){
        try {
          div.divide(0, 0);
          Assert.fail("Expected divide by zero error");
        } catch (Exception e) {
          // Expected
        }
        Assert.assertEquals( 0, div.divide(0, 10));
        Assert.assertEquals( 3, div.divide(10, 3));
        Assert.assertEquals( 0, div.divide(3, 10));
        Assert.assertEquals( 1, div.divide(3, 3));
      }
    }

I came up with a few implementations (hence the interface, makes testing much
easier) but I think this one is the cleanest. I'm pretty sure there's a
solution using bit shifting but let's face it, most developers don't use bit
shifting in their code.
