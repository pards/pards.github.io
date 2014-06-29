---
layout: post
title: Dysfunctions of a corporation
categories:
- People
tags: 
- Organizational Structure
---

Deploying a sufficiently complicated application into a corporate environment
involves navigating a sea of paperwork, getting all the appropriate
"approvals" in place, co-ordinating with department managers to get time slots
from their staff, and so on.  

### Knowledge silos

On Release Day all the right people from the various departments - DBAs,
application server administrators, server administrators (both Windows and
Unix), desktop support, storage experts - are all sitting in their cubicles at
the allotted time waiting to check their checkbox, waiting to mark their task
as Done, so they can catch the 5:31pm train home.

These distributed silos of expertise have no vested interest in actually
getting the application working. They follow their prescribed steps to Done
and hand over to the next department in the chain. Each department only sees a
small cog in the machine.

Much finger-pointing ensues when problems arise. The people charged with
resolving the issues (usually the application team) are at the mercy of the
experts, because only the experts are permitted to view the logs, or monitor
processes, or produce query plans.

### Self-contained teams?

Ideally the application team would contain all the expertise it needed rather
than relying on people outside the team, and these experts-within-the-team
would have the power (and responsibility) to manage the production deployment
themselves.

Self-contained teams can be challenging to assemble in large companies because
often their IT departments are organised by technical expertise rather than
functional responsibility. Getting allocated 20% of a DBA is not the same as
having one sitting in the project room.

### Automate, automate, automate

If the deployment can be expressed as a set of verifiable steps then why not
build it in code? The deployment would be repeatable and self-documenting, and
would not rely on resources outside of the application team - except maybe to
provide temporary passwords.

Deployment automation makes people in large corporations understandably
nervous. There's a trust issue, and job security concerns, so I'm proposing
the next best thing: a Flash Team.

### Flash team

A Flash Team is my way to describe a cross-departmental group of people
brought together for a short period of time to solve a particular issue, or to
achieve a goal. Everyone involved in the deployment gathers in one room where
they work _together_ to get the whole system up and running smoothly. If
problems crop up then the development team can pair with the expert to resolve
it. The experts see the big picture, the development team gets unfettered
access to the right people, and the application gets deployed.

Who knows, they might even bond over a pizza when it's Done.

