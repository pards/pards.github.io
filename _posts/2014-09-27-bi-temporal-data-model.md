---
layout: post
title: "Bi-temporal Data Model"
description: ""
category: database
tags: []
---

A bi-temporal data model is a fancy way of saying that all tables have 2 pairs of dates: business from/to, and audit from/to.  The business dates track expected day-over-day changes in business data such as the varying quantities of widgets in stock.  The audit dates are used to track when the data was loaded into the database, and any data fixes applied by IT.

In essence, every table contains the current record as well as the full log of all the changes that were ever made to that record.

The customers love this idea. "You mean I don't need to have a separate data warehouse? Great!"

A bi-temporal data model is occasionally used to try to address the [Basel III](http://en.wikipedia.org/wiki/Basel_III) accord because it allows you to show how the data looked at any particular point in time without having to restore from backup or go to a separate database.  The belief is that you could re-produce a report exactly as it was originally produced but this would only work if the code used to produce the report was also stored in a bi-temporal fashion.

Anyway, on with the implementation.

Bi-temporal queries
-------------------

Correctly implementing application code to work with a bi-temporal data model is surprisingly difficult. For starters, every database query has to specify both sets of dates _on every join_ in the query, which can quickly bloat even the most straightforward of queries.

For example:

    select *
    from order o
    join currency c on c.currency_id = o.currency_id;

becomes:

    select *
    from order o
    join currency c on c.currency_id = o.currency_id
    and c.business_from <= 'bus_date' and c.business_to > 'bus_date'
    and c.audit_from <= 'audit_date' and c.audit_to > 'audit date';
    where o.order_id = 123456
    and o.business_from <= 'bus_date' and o.business_to > 'bus_date'
    and o.audit_from <= 'audit_date' and o.audit_to > 'audit date';

Add a few more tables and things become pretty unwieldy.

Believe it or not, querying a bi-temporal data model is the easy part.  Updating and deleting data is where things get weird.  You can't just update or delete a row, you need to do it bi-temporally.  

Bi-temporal deletes
-------------------

To delete the record, one has to 

1. Update the existing entry, setting the 'audit_to', and then,
1. Insert a new entry with a new 'business_to'.

Sticking with our currency example from above.

Here's the original record

| currency_id | currency_cd | descrption      | business_from | business_to | audit_from       | audit_to |
| ----------- | ----------- | --------------- | ------------- | ----------- | ---------------- | -------- |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | null        | 2000-01-01 15:00 | null     |


And here's how it should look after a bi-temporal delete:

| currency_id | currency_cd | descrption      | business_from | business_to | audit_from       | audit_to         |
| ----------- | ----------- | --------------- | ------------- | ----------- | ---------------- | ---------------- |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | null        | 2000-01-01 15:00 | 2014-09-27 17:30 |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | 2014-09-26  | 2014-09-27 17:30 | null             |


Bi-temporal updates
-------------------

There are two types of updates we need to consider

- A business change
- A data correction

### Update (business change)

This scenario is when the data has changed from the business' point-of-view.  This requires 3 separate operations

1. Update the original entry, setting the audit_to
1. Insert a new entry that's a copy of the original, but setting the business_to
1. Insert a new entry that reflects the new data

Before:

| currency_id | currency_cd | descrption      | business_from | business_to | audit_from       | audit_to |
| ----------- | ----------- | --------------- | ------------- | ----------- | ---------------- | -------- |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | null        | 2000-01-01 15:00 | null     |

After:

| currency_id | currency_cd | descrption      | business_from | business_to | audit_from       | audit_to         |
| ----------- | ----------- | --------------- | ------------- | ----------- | ---------------- | ---------------- |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | null        | 2000-01-01 15:00 | 2014-09-27 17:30 |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | 2014-09-26  | 2014-09-27 17:30 | null             |
| 123         | CAD         | Cannuck Cash    | 2014-09-26    | null        | 2014-09-27 17:30 | null             |

### Update (correction)

This scenario is when the data needs to be corrected because it was incorrect when it was added to the database. For example, the currency data file was loaded correctly but contained errors that now need to be corrected manually.

This requires 2 separate operations

1. Update the original entry, setting the audit_to to 'now'
1. Insert the corrected entry, setting the audit_from to 'now'


Before:

| currency_id | currency_cd | descrption      | business_from | business_to | audit_from       | audit_to |
| ----------- | ----------- | --------------- | ------------- | ----------- | ---------------- | -------- |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | null        | 2000-01-01 15:00 | null     |

After:

| currency_id | currency_cd | descrption      | business_from | business_to | audit_from       | audit_to         |
| ----------- | ----------- | --------------- | ------------- | ----------- | ---------------- | ---------------- |
| 123         | CAD         | Canadian Dollar | 1858-01-01    | null        | 2000-01-01 15:00 | 2014-09-27 17:30 |
| 123         | CAD         | Cannuck Cash    | 1858-01-01    | null        | 2014-09-27 17:30 | null             |

Conclusion
-------------------

The data architect responsible for the data model for my current client regularly holds "how to use the data model" meetings to explain these concepts to the developers and business users.  If your data model requires an explanation then it's a bit of a problem. There are often 20 or more people in these meetings which they typically go for an hour. But what happens when the data architect takes a new job? 

Inevitably people will make mistakes when working with the data, and all it takes is a single bi-temporally incorrect record to break an application that may get multiple rows back from a query that should have returned one.

A bi-temporal data model increases the code complexity by at least an order of magnitude, and this additional complexity makes it much slower to implement, and even longer to test.  It also requires developers that are very diligent and resist the urge take short-cuts which might "break the model". 

In my opinion, you should avoid using a bi-temporal data model if you can.  It sounds great, and sells well, but it will cost the project dearly, and might even cause its demise.  You'd be much better off building a separate data warehouse for your historical data and keeping your "current" data simple.  

I thnk the only person that benefits from a bi-temporal data model is the data architect because he/she becomes indispensible. The developers have to produce greater volumes of complex code, the timelines get pushed out, and the business doesn't get the stable system that they paid through the nose for.

Keep it simple, create a data model that people can understand, and your stakeholders will thank you.