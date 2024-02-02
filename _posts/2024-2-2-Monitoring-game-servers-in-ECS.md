---
layout: post
title: Serverless Palworld Adventures - Spying on a Palworld server in ECS
---

As part of trying to setup a serverless Palworld server, I needed to be able to monitor it.  I have to know if the server comes up properly, and once it's up, if players are connected or not.

Unfortunately I can't be like Lifmunk here and just demand it!

[Lifmunk_uzi]({{ site.baseurl }}/images/palworld_lifmunk_uzi.png)

## How I've done it in the past

So with the Minecraft server I run, I used the method found [here](https://github.com/doctorray117/minecraft-ondemand/blob/main/README.md), which taught me a bunch, and works very well.  Prooooblem is that it only works for TCP connections, by using two containers in 1 task (the game server, and a sidecart to monitor it), allowing both to have access to the same network adapter.  Meaning that you can use netstat to show ports listening, and also active TCP connections (from players using the server).  Unfortunately Palworld uses UDP, making this moot.  So I had to come up with a different solution.

## What did I try?

Well previously I've used [Gamedig](https://www.npmjs.com/package/gamedig) which works well too, send the command, check for correct output, and you're off to the races.  Problem is that Gamedig's got some bugs with regard to Palworld and isn't working!

![my_tantrum]({{ site.baseurl }}/images/i_want_it_now.png)
*I may have re-enacted this...*

I then tried to adapt the TCP method above, figuring that while I can see the UDP listening, I can't see active connections, but I can see when UDP comes in from an outside source, only for a limited time.  There is a way to modify the timer before the UDP entries are removed from the list, but I was concerned modifying it might cause issues with the other container as they're sharing the adapter.

## So what did I do in the end?

When configuring the container for Palworld, I remembered seeing the setting for enabling RCON.  RCON was developed by Valve as a way to get a Remote CONsole, hence the name.  You can find more information [here](https://developer.valvesoftware.com/wiki/Source_RCON_Protocol)

After a bit of searching online, I found a neat little RCON CLI client called [ARRCON](https://github.com/radj307/ARRCON).  Works a treat, but did require some modifications to get it in.  I had to modify the sidecart to install that instead of Gamedig

![sidecart]({{ site.baseurl }}/images/sidecart_docker.png)
*nothing complex, download, extract and make it executable*

Once that was done, I then had to get my sidecart container to loop checking if RCON was active, thereby signalling the server was up, then start checking player count once a minute and trigger the shutdown if no players were online for x loops.  You can see the code [on my Github](https://github.com/joebywan/GameServerIaC/blob/main/palworldecswatchersidecart/palworldsidecart/watchdog.sh)

## What did I learn?

UDP's a real pain in the behind to get visibility into via the network.  Thankfully there can be queries in place to check those servers, only if the devs implement them.

Considering the cost of the extra cpu/ram for the sidecart, and that Lambda's free teir allows 1,000,000 executions and 400,000GB seconds free per month, I've been contemplating implementing a centralised Lambda instead of the sidecart.  It would mean opening a port on the server to the VPC only, but it's firmly on the backburner list.

## What's next?

- I'm still finalising the Terraform module for the server itself
- Setup Cloudwatch>SNS>Lambda to restart the server if it's resource usage gets too high.  I also want to use RCON to send messages to players on the server informing them of the upcoming restart.
- Setup SNS>Lambda>Discord so I can be pinging my discord server when servers go up/down/restart.  Means I don't have to subscribe all of my players emails, just invoke 1 Lambda and they can monitor the Discord for info.

As usual, happy for suggestions/advice/criticisms on how I'm doing it, welcome the feedback and chance to learn more from you!
