---
layout: post
title: Serverless Palworld Adventures Part - Subscription Filters
---

Bare with me while I wander off topic a little.  Myself and all of my friends with basically anyone we know is playing a game called Palworld right now.  I wanted to setup a dedicated server on the cloud to avoid home internet issues, the server being on etc.  The solution I've used previously uses AWS CloudWatch Logs Subscription Filters.  In working on this, I realised that AWS only allows 2 filters per CloudWatch Log Group.

So... how are we going to make this work anyway??

# Why am I doing this?

The server I want to setup is running on Elastic Container Service(ECS) and setup to only be running (and costing me $$$) while it's in use.  It turns off automatically, but also powers on whenever someone tries to use it.

To do so, I need logs from whenever my domain on AWS Route 53 is looked up to trigger automation to start the server... I was doing this with query logs for the hosted zone to CloudWatch with Subscription Filters, which I now know isn't a scalable solution due to restrictions.

# So how am I working around it?

For something to look at instead of read, here's a really basic layout of it:

![Picture_Example]({{ site.baseurl }}/images/subscription_filter_router.png)

To start with, AWS won't increase the limit.  So I figured if they weren't going to do the filtering for me, I'll have to.  Enter stage left... Lambda!  It's serverless, free tier means it'll cost me nothing, and it'll do the thinking and routing of actions.  So we'll setup 1 Filter to rule them all.  

Using a filter pattern like so: \[,,,url="minecraft.test.com" || url="valheim.test.com" || url="palworld.test.com",...\] this will grab the server dns queries if people attempt to use it, and forward it to Lambda.

Lambda then is responsible for parsing the logs, which was a puzzle in itself.  AWS helpfully (not) sends the log entries encoded in Base64 ASWELL as Gzip compressed.  So a couple of steps and loading the json gets it into python.

I can then pull the URL that was queried, and use the subdomain that was sought as a key for a key value dictionary supplied, outlining the ECS Cluster, Service and AWS Region to modify the desired tasks to 1 from 0.  Voila, it's ALIVE!

So while I'm happy my problem's solved, it's a little meh in that I now have to do two seperate deployments.  One for the filter router, and then another for each server.  Thankfully, I'm building it all in Terraform, making it easy to update, modify, tear it down, build it up.

There'll be more, I'm keen to get this sorted, so I can focus on playing with my mates.  Future installments are likely to discuss:

- Setting it up to automatically update Route53's DNS entry with the dynamic ECS Task IP
- Automatically detecting whether the Task is in use or not
- Communicating with a server via RCON using Bash or Python
- Monitoring it, it's early access, and eats more memory than Chrome.  Lets implement automated restarts to avoid out of memory crashes
- Sending notifications to Discord to let everyone know the server is up

Stay tuned!  If there's anything you want to see about it, hit me up on LinkedIn and I'll share :)
