---
layout: post
title: "Sharing Databases"
description: ""
category: database
tags: []
---

At [Intelliware](http://intelliware.com), each developer runs the database on their workstation. By that I mean that everyone has a copy of Oracle, Postgres, or whatever installed on their machine with all the tables and data that the application may require. This allows us to work independently of each other, and even work offline in case Armageddon strikes. But it also has much more important benefits.

I'm currently working at a client site where the developers don't run the database on their workstation so I can now fully understand the importance of this seemingly minor setup preference. 

Having their own database frees developers from the fear of breaking other peoples' work.  If I accidentally drop a table the only person that will be impacted is me.  Furthermore, if my code has a bug that messes up the data then I'm not going to take other developers offline, which is huge from a team productivity point-of-view.

But the killer benefit is reproducibility. 

By definition, database changes have to be scripted, automated, and checked-in to source control so that when other developers pull down the latest codebase they can migrate their database easily. In other words, we get very good at rebuilding the database in a consistent and predictable manner.  When it comes time to ship the code to another environment then the database upgrade scripts have already been thoroughly tested on the developers' workstations and the installation goes smoothly.

In a shared database environment, database changes (whether schema or data) can be managed in a couple of different ways: Control Freak, or Anarchy. There may be other "techniques" but I've only seen these two.

In a Control Freak environment the developers have no privileges on the database.  They can't create/modify/drop tables, or tune them with indexes, create stored procedures (which is probably a good thing anyway) or do any of the admin-ish tasks.  Instead, they have to ask a DBA to do it for them.  This approach slows down development and decouples the versions of the db schema and the code, and creates a bottleneck/dependency with the DBA group.

In Anarchy the developers have all the privs under the sun.  They can move quickly and fluidly, but their changes may not be reproducible due to the lack of process.

In both Control Freak and Anarchy, all developers need to upgrade their code whenever someone pushes a database-related change.  This is a Bad Thing in my opinion because it means that any significant change can take the team offline until the issues are resolved.


