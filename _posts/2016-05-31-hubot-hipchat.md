---
layout: post
title: "Hubot and HipChat"
description: "Running Hubot with an on-prem HipChat server"
category: bot
tags: [hubot, hipchat]
---
Configuring [Hubot][1] to work with a self-hosted [HipChat][3] server took a bit of tweaking, so here's a quickstart guide based on what I discovered along the way.

First of all, follow Atlassian's guide to create a copy of Hubot.  This will download the source code including the HipChat adapter.

    $ npm install -g yo generator-hubot
    $ mkdir myhubot
    $ cd myhubot
    $ yo hubot --adapter hipchat

Answer the questions, and you've got a basic bot config

Next up, you'll need a HipChat user.  Ask your ~~parents~~ HipChat admin to create one for you.

Hubot is configured using environment variables (i.e. export VAR=value).  I created a script named set-env.sh to set these for me

	#!/bin/bash
	# The username is NOT the user id, it's the JabberID from this page
	# See https://your.hipchat.server.com/account/xmpp
	export HUBOT_HIPCHAT_JID=1_1234@chat.btf.hipchat.com
	export HUBOT_HIPCHAT_PASSWORD=

	export HUBOT_HIPCHAT_ROOMS=1_sandbox@conf.btf.hipchat.com
	#export HUBOT_HIPCHAT_ROOMS_BLACKLIST=
	export HUBOT_HIPCHAT_JOIN_ROOMS_ON_INVITE=true
	export HUBOT_HIPCHAT_JOIN_PUBLIC_ROOMS=false

	export HUBOT_HIPCHAT_HOST=your.hipchat.server.com
	export HUBOT_HIPCHAT_XMPP_DOMAIN=btf.hipchat.com
	export HUBOT_LOG_LEVEL=debug
	export HUBOT_HIPCHAT_RECONNECT=true

There are a couple of noteworthy points here
	
**Gotcha #1** The `HUBOT_HIPCHAT_JID` is NOT the username you log in with; it's the Jabber ID.  You can find this in HipChat via the web UI by

1. Log in as the user
1. Click on your avatar in the top right corner
1. Choose 'Account Settings' from the drop-down
1. Choose 'XMPP/Jabber info' from the navigation pane on the left
1. The value in `Jabber ID` is what you want. It probably looks like 1_1234@chat.btf.hipchat.com.

**Gotcha #2** The `HUBOT_HIPCHAT_HOST` is your on-prem/self-hosted server name. The one that you'd type into the browser.

**Gotcha #3** The `HUBOT_HIPCHAT_XMPP_DOMAIN` *must* be set to `btf.hipchat.com`.  If you don't do this, then your bot will only work with 1:1 chats, but won't be able to post to a room.

**Gotcha #4** The `HUBOT_HIPCHAT_ROOMS` syntax is weird. I have a sandbox room that my bot connects to automatically.  There are a number of ways find out what the address is on your installation, but the easiest is to turn on debug logging then invite the bot to join your room.  The bot will log the room name.

Ok, now you're ready to take your bot for a spin.

    $ bin/hubot -a hipchat

First up, try a 1:1 conversation.  There are a few commands that work by default so try a basic "ping" and your bot should respond with PONG.

Next, invite the bot to a _quiet_ room then try "@BotName ping".  The Ping command uses `robot.respond` so it requires the bot's name for it to work.

Now, go forth and code your bot! See the [scripting docs][2] for a quick overview on the syntax.

[1]: https://hubot.github.com/ "Hubot"
[2]: https://hubot.github.com/docs/scripting/ "Hubot docs"
[3]: https://www.hipchat.com/ "HipChat"
