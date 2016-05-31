---
layout: post
title: "Keep Everything in Source Control"
description: ""
category: agile
tags: [agile, devops, continuous delivery]
---
> Me: Why is this report different to the one in production?
> SysAdmin: Dunno. I guess they must have fixed it production and forgot to check it back into source control.

If I had a dollar for every time I've had that conversation I'd be a rich man.  It is usually the result of some late-night troubleshooting and finally the system is working as expected and everyone goes to bed. But the next day, nobody remembers what exactly they changed that fixed the problem, nor do they spend the time to figure it out and get it back into source control.  Worse yet, they were making untested changes in a live environment - _that shouldn't be possible!_.

Gone are the days when source control was just for source code.  Everything required to build, package, deploy, migrate and run your application should be kept in source control.  That includes source code, app configs, build scripts, deployment jobs, SQL scripts (DDL and data), server settings, smoke tests ... *everything*.

The fact that this team was troubleshooting a release in production tells me that their deployment process is probably abysmal, so it's easier to just change things in-situ than to go through the whole commit-push-deploy cycle again.  It's tempting to blame the people involved but more often than not this is an organisational problem. If it was easy to push new builds through the test environments and to production then then tech team would do that.  

Deep down, people want to Do The Right Thing but in a low-trust organisation that means days or weeks of paperwork, sign-offs, release approval meetings and other forms of Release Management Theatre. A heavy, approval-based release process like that actively discourages the tech team from locking down production because if the only way to get code to production is through source control then they're screwed if something goes wrong.  The system will be offline for days or weeks while they wait for the sign-offs.

And then bugs that were fixed through the heroic efforts on the live servers get re-introduced on the next release because they never made it into source control. Or worse yet, the team does patches instead of full releases so that a few cycles later nobody has any idea if they can re-create the production instance from source control.  Spinning up new instances involves copying the whole filesystem.

If this sounds like your environment then take a long, hard look at your release governance process.  Does it encourage and facilitate rapid deployments?  If not, you should start automating the release process for all environments so that production releases become boring and predictable.  Start by measuring how long it currently takes from a "go live" decision to the code running in production (including QA testing and sign-offs). Now automate the longest step. Repeat.

Do this with diligence and you'll have a reliable, reproducible system that can push fixes on short notice without relying on [Brent(s)][1].

[1]: https://www.amazon.ca/Phoenix-Project-DevOps-Helping-Business-ebook/dp/B00AZRBLHO "Don't be Brent"

