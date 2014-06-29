---
layout: post
title: Synchronized code in distributed systems
categories:
- Code
tags: 
- Java
- Distributed
- Concurrency
---

I encountered some code recently where a GUI client was listening for Object
updates from a multi-threaded server, and it was using the version number on
the Object to determine whether the incoming Object was the latest version.
The server incremented the version of the Object by inserting a new row into a
table in the database and using the primary key as the version number.

Now consider the case multi-threaded case where two threads, T1 and T2, are
making updates to the Object.

  1. Both threads start a transaction.
  2. T1 increments the version to v1.
  3. T2 increments the version to v2.
  4. T2 commits and sends the notification to the client.
  5. T1 commits and sends its notification to the client.

The GUI ignores the update from T1 because it already has v2 of the Object
from T2. The problem was that the changes from T1 were not included in the
update from T2 because T1 had not committed its database changes.

The underlying problem here is that there is no synchronization around
incrementing the version. There are several ways of solving this problem:

**Option 1**: use Java's synchronized keyword  
In this solution the version number would be incremented in a synchronized
block, using the Object instance as the lock on which to synchronize.

    synchronized(myObject) {
      myObject.setVersion( myObject.getVersion() + 1);
    }
    
This method works in a multi-threaded environment but does not work when the
application is clustered across multiple servers.

**Option 2**: use the database as the synchronization point.  
In this solution the version number is moved from its own table to the master
record, and the field is updated when we need to increment the version.
However, there is a pitfall that needs to be avoided for this to work as
expected.

The incorrect way to do this is to increment the value in Java and save it.
    
      String sql = "update my_obj set version = ? where id = ?";
      ...
      stmt.setInt( idx++, o.getVersion()+1);
      stmt.setInt( idx++, o.getId());
      stmt.execute();

The problem here is that different threads could write the same version number
when they should have incremented it. Consider this scenario:

  1. Both threads start a transaction.
  2. T1 reads the Object (v1).
  3. T2 reads the Object (v1).
  4. T1 increments the version to v2.
  5. T2 increments the version to v2.

T2 should have incremented the version to v3 but T1 hadn't committed its
update when T2 read the record from the database.

The correct way to do this is to update the record in place because this
forces all threads across all clustered servers to get a write lock on a
single record in the database, and the value that they are incrementing is
always the latest. Each thread will block until the previous one has committed
its change.
    
      String sql = "update my_obj set version = version + 1 where id = ?";
      ...
      stmt.setInt( idx++, o.getId());
      stmt.execute();
