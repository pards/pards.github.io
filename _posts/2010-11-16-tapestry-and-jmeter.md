---
layout: post
title: Tapestry and JMeter
categories: 
- Web Development
tags: 
- Tapestry
---

I was recently tasked with setting up some load tests for our Tapestry (4.1.6)
web application. I chose to use [Apache JMeter](http://jakarta.apache.org/jmeter/) because it is open-source and is
flexible enough to do what we needed, and doesn't cost $12,000/month like some
load testing tools.

I set up an HTTP Proxy Manager to record a simple navigation through the site
and reconfigured my browser to use the proxy. The proxy recorded the requests
without a hitch.

Then I hit a hurdle. A number of our pages use @Persist("client") for a few
values that need to be retained between server conversations. Typically this
is so that the pages keep their context across multiple requests. Tapestry
encodes these values into the URL meaning that the data recorded by the proxy
server isn't re-usable across different environments and users.

For POSTs to work I needed to extract these encoded parameters from the GET
results and send them back with the POST. Enter the Regular Expression
Extractor. I added a suite-wide extractor for the 'seedids' parameter that
looks like this

	Reference Name: seedids  
	Regular Expression: (name="seedids")(.+?)(value=")(.+?)(")  
	Template: $4$  
	Match No: 1  
	Default Value: REGEX_FAILED_SEED_IDS  

For the @Persist("client") parameters I added a page-specific reg-ex
extractor. I needed one per page because the parameter name changes each time.
The extractor for the "state:path/to/page" parameter looks like this

	Reference Name: state  
	Regular Expression: (name="state:path/to/page")(.+?)(value=")(.+?)(")  
	Template: $4$  
	Match No: 1  
	Default Value: REGEX_FAILED_STATE  

One gotcha: I had to use "Body (unescaped)" in the regex extractor to get the
real value for these.

Now I just replaced the recorded seedids and state values with ${seedids} and
${state} (with encoding enabled) and my POST requests worked like a charm.
