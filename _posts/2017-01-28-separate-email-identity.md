---
layout: post
title: "Separate Your Email From Your Identity"
description: "Separate Your Email From Your Identity"
category: identity
tags: [identity, email]
---

Your email address identifies you in this online world.  Companies use it to verify who you are, to communicate with you and for resetting passwords. You may also use the same email address to correspond with your friends and family, and it may even be linked to your calendar too.

It makes sense to separate your personal email from your work email - you don't send personal emails from your work address, nor do you send work emails from your personal address.  In all likelihood you use a free service like Gmail, Hotmail, AOL or Yahoo for your personal email.

Now imagine what could happen if you lost control of your email account. Maybe you got hacked. Maybe you use the same password for everything and it got leaked from an insecure website (check [Have I Been Pwned](https://haveibeenpwned.com/)). Maybe Google decided to [lock your account](https://www.engadget.com/2016/11/17/google-blocks-pixel-phone-resellers/) - after all they're [not under any obligation](https://www.google.com/intl/en/policies/terms/) to continue offering the service.  

The hacker can now log into your email account.  They search for any banking-related emails, figure out who you bank with, go to the bank's online banking website and click the 'forgot my password' link.  

Now they own your bank account too.  They repeat this process for your investment account, your mortgage provider, your phone and internet providers, electricity, gas, maybe even the tax department.

Before you know it, you've lost control of everything that matters. All through email.

You need to separate your email from your identity so that you own and control your online identity. The easiest way to do this is to set up a private email address on your own custom domain and use this new email address as the contact email for any service that matters to you (but not for anything else).  

You could save yourself the time and effort and just enable two-factor authentication on your Gmail account to reduce the chances that you'll get hacked, it doesn't leave you with any recourse in the case where you actually get hacked, or if Google locks your account.  With your own domain, you could change the DNS records and switch to a new provider and cut off the hacker's blood supply in a matter of minutes.

Here's how to go about it:

1. Buy a domain name. It doesn't matter what it is but make sure you _pay_ for it so its ownership is not questionable. I use [NameCheap.com](https://www.namecheap.com/?aff=109861) for this because their prices are reasonable and I find their user interface is easier to use than most.

1. Register with a paid email provider that supports custom domains.  [ProtonMail](https://protonmail.com/pricing) and [FastMail](https://www.fastmail.com/?STKI=14088017) both offer this service for a reasonable price.  ProtonMail has the added benefit of being both open source and encrypted on the server so nobody can read your email even if their servers get hacked. Both have excellent websites and mobile apps. 

1. Follow the instructions from the email provider for setting up your custom domain. See [here for FastMail](https://www.fastmail.com/help/receive/domains.html) and [here for ProtonMail](https://protonmail.com/support/knowledge-base/set-up-a-custom-domain/)

1. Go to each service that matters and change your email address.  Start with online banking and other sites that have your financial data, then move onto other life essentials like utilities. 

1. Set up two-factor authentication with all of them - NameCheap, FastMail, and ProtonMail all support it.

1. Bonus Step: Use a password manager like Enpass or LastPass to use generated passwords for those services.  It's worth paying extra to make sure your passwords are sync'd across all of your devices because it'll reduce the risk that you'll go back to using a memorable password.

... and don't even get me started on [social logins](https://en.wikipedia.org/wiki/Social_login); they're a single-point-of-failure for the security of your digital identity.
