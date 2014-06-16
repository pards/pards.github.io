---
layout: post
title: Reflection HashCodeBuilder Performance
categories:
- Software Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _aioseop_description: HashCodeBuilder is a great utility from Apache Commons, but
    be wary of the performance implications of ReflectionHashCodeBuilder.
  _aioseop_title: ReflectionHashCodeBuilder Performance
  Title: ReflectionHashCodeBuilder Performance
  Description: HashCodeBuilder is a great utility from Apache Commons, but be wary
    of the performance implications of ReflectionHashCodeBuilder.
  Keywords: ReflectionHashCodeBuilder Performance, HashCodeBuilder Performance, hashcode
    and equals
  _aioseop_keywords: ReflectionHashCodeBuilder Performance, HashCodeBuilder Performance,
    hashcode and equals
author:
  login: craig
  email: craigpardey@gmail.com
  display_name: craig
  first_name: Craig
  last_name: Pardey
---

I recently embarked upon a performance investigation to figure out why a
particular operation was slow. My theory was that it was the
serialization/deserialization cost with a set of large objects; my colleague's
theory was the iteration over the set. We were both wrong.

The investigation became intriguing when we started measuring the elapsed time
at various points in the application and noticed that the bulk of the time was
being spent in a seemingly innocuous call to construct a HashSet from an
existing Collection.

Knowing how Java works under the hood is invaluable in diagnosing these
issues. Java will make heavy use of hashCode() and equals() when constructing
a Set because it needs to determine uniqueness. When we checked the hashCode()
and equals() implementations in the class of the set we found that it was
using the [Apache Commons](http://commons.apache.org/lang/api/org/apache/commo
ns/lang3/builder/HashCodeBuilder.html#reflectionHashCode\(java.lang.Object,
boolean\)) utility classes.

    
    
    
    @Override
    public int hashCode() {
      return HashCodeBuilder.reflectionHashCode(this, false);
    }
    
    @Override
    public boolean equals(Object o){
      return EqualsBuilder.reflectionEquals(this, o, false);
    }
    

This is a pretty lazy way to implement these methods. In an Agile context, it
probably falls into the "do the simplest thing that could possibly work"
category, but the "proper" implementation would have only taken a few extra
minutes and would have saved us a lot of investigation time, and would have
kept our users happy.

I baselined this implementation against an explicit implementation and found
that the reflection version was **5 times slower** on a simple object. In our
particular case it was several orders of magnitude slower because of the
complexity of our Object graph.

But it gets even better. The object that was using the reflection hashCode
builder had a unique identifier that we could use, so implementing an explicit
hashCode builder took our run time down to almost zero.

    
    
    
    @Override
    public int hashCode() {
      HashCodeBuilder b = new HashCodeBuilder();
      b.append(getId());
      return b.hashCode();
    }
    
    @Override
    public boolean equals(Object o){
      if( o == null){
        return false;
      }
      if( ! (o instanceof MyObject)){
        return false;
      }
      MyObject that = (MyObject)o;
      EqualsBuilder b = new EqualsBuilder();
      b.append(getId(), that.getId());
      return b.isEquals();
    }
    

The sample code is available on [GitHub](https://github.com/pards/hashcode)

