---
layout: post
title: "Thoughts on Netezza"
description: ""
category: rant
tags: [Database]
---

I've been using Netezza for a few months now and this post captures my opinions based on my limited experience. For context, I'm populating a Netezza database for a client. The database schema is managed by the client, and Netezza was selected prior to my involvement. Netezza is their data warehouse, but they're also using it a little like an application database.  They're using a [bi-temporal model](http://en.wikipedia.org/wiki/Temporal_database), but that's a subject for another post.

Let's begin on a positive note.

Netezza is great at bulk loading data - it can suck in millions of rows per second. The external table syntax is simple and concise, and allows external tables to be defined as "same as" another table which removes all sorts if duplication problems. Netezza is also pretty fast for querying, especially over very large datasets. In short, Netezza is great as a data warehouse platform.

It can handle huge datasets without breaking a sweat.

But it is painfully slow at row-by-row insertions. It would almost be faster to type the data in by hand, or send it Snail Mail.  Anything more than a few dozen rows and you can either go get a coffee or cut over to bulk loading.

Netezza has no meaningful support for constraints. Primary key constraints are ignored. Foreign keys? Ignored too. Unique constraints? Bzzt. In fact, the only constraints that I've seen it enforce are non-nullable columns. Constraint support may not matter to some people, but I like databases that police the integrity of my data in the same way I like using programming languages with a static type system. It prevents mistakes before they happen. 

One of the coping mechanisms for dealing with the lack of constraint checking is to write automated processes that scan the database looking for violations. But it begs the question, "Why should we have to do this?". Any reasonable RDBMS does this out-of-the-box. A custom, hand-coded solution can identify the constraint violations but the erroneous records may already be linked to millions of other rows, so correcting the problem becomes a headache.

And then there's sequences. The underlying hardware architecture means Netezza has to allocate blocks of IDs to each SPU. Using small increments makes them slow, but large increments can quickly exceed the data size of the ID column, particularly if the server needs to be restarted.

To get around this, some developers start using complex SQL to create the identifiers based on `max(id) + rownum`.  This approach works fine in a single-user environment but can cause duplicates when two queries hit the same table at the same time.  But of course, Netezza won't complain about duplicate keys so you won't know unless you check for it.

Keep in mind that Netezza is an appliance. It's a physical piece of hardware. IBM has released a [software emulator](http://tinyurl.com/lr2uljz) for development purposes but it is a Virtual Machine that requires a fair bit of grunt to run (4 cores, 64-bit, 4Gb RAM).  It has served us well, but it's not quite the same as being able to fire up an instance of PostgreSQL or Oracle.

Netezza is *not* a drop-in replacement for Oracle or PostreSQL. It is not advertised as such, but it's support for Big Data plus SQL make a compelling case to use it as an application database.  Do not do this; it is the path to hell. IT management will try to push for this to maximize their ROI, but it will cost a lot more in the long run.

This post turned into a bit of a rant, and I apologise for that, but I had to get it off my chest.

