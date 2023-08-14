---
layout: post
title: How I learnt more about AWS serverless while trying to be a miser and save some coin
---
# Before we get into it
This is a bit of a brain vomit/stream of consciousness, writing it as a I play with things and overcome obstacles.  This will probably get a bit rambly, but I hope the thought processes help others out.  You've been warned!
![stream_of_consciousness]({{ site.baseurl }}/images/stream_of_consciousness.jpg)

# The problem
I'd been told a while bac the M1 Mac's couldn't use more than 1 external monitor, so I got myself a nice 34" ultrawide Dell monitor.

Then I was informed by a coworker that we've got M1 Pro's! I'd never bothered to check (facepalm).

I now want to find another one, but it's a want, not need situation, so I'd like tickle my inner miser to save some coin.

Enter stage left... Serverless!
![Cost optimisation!]({{ site.baseurl }}/images/cost_optimisation.png)
# The solution

## Well... initially
I'm a big fan of ChatGPT, use it to do the grunt coding, which I couldn't be bothered doing.  I got it to slap together a python script to check the dell site for the monitor, [Link](https://www.dell.com/en-au/shop/dell-34-curved-usb-c-monitor-s3423dwc/apd/210-beic/monitors-monitor-accessories).

Now it gave me a script, I'd heard of Beautiful Soup(BS) before, but not used it.  I asked ChatGPT to tell me how to find the information I'd need to retrieve just the price.  It wanted to know the current price, the url and selector.  So I'm asking it how to fill out the selector.

It gave me the instructions to right click the price, hit inspect and use the values there.  I have to admit I wasn't feeling like sussing out the BS search syntax, so I just grabbed the html around it, threw it into ChatGPT and it gave me the code.  Ran it and it worked!

## Longer term thinking
Problem is that it was using a 1hr sleep on a loop on my machine.  I'd much rather use AWS's instead so I can turn mine off.  at nearly 30c/KWH it's cheaper to use AWS services for this stuff than run my machine.

I slapped together some terraform I'd used in a previous API gateway experiment, changed it to use the current lambda function and was off to the races.  Hit the API gateway endpoint and no luck.

## Get logging
End up digging further into how to enable logging on Lambda & API gateway via Terraform, which was straightforward for Lambda, but API Gateway... In Terraform, have to setup the CloudWatch log group, but providing permission to API Gateway was not how I thought.  I was using API Gateway V2, and all of the TF resources for APIV2 are seperate.  In V1 there's a "aws_api_gateway_account" resource.  In V2, there's nothing similar.  I searched for a while and couldn't find anything, gave it a shot and it worked.  IMO should be duplicated into the V2 area aswell if it's usable for both, or have a seperate 'common' resources to both versions area.  Anyway... Logging's up and working.  Unfortunately Lambda's shagged.

## Lambda woes
Fix some syntax errors with my favourite AI LLM shoulder parrot and then was presented with an error that BS wasn't able to be imported... Wellll crap.

I'd read about packaging modules with Lambda during my cert studies, but hadn't had an opportunity to use it in my day to day work responsibilities yet.

With some back and forth on how to do it, I ended up doing the following:
1. Setup a requirements.txt with the modules I needed
1. Build a directory containing all of those modules by using `pip install -t requirements.txt -r ./lambda_package/`
1. Copy the lambda script into the directory
1. Reconfigure TF to zip the new directory and upload to Lambda instead of just the script

Thankfully after that I had a successful test!

I already had one problem I'd been putting off dealing with, which was that TF's archive_file resource didn't always recreate the zip file when I updated the script.  Now I had a 2nd problem, icky manual tasks required for packaging up the Lambda function.

## Automate the manual deployment stuff
I figured I've been automating everything else, lets get this package build automated.

The options that came to mind were:
* Bash script
* At work we use the [3 Musketeers deployment method](https://3musketeers.io/) which Make is a big part of.  Figured Make should do it.

I decided to go with Make as I'd not built something from scratch before using it.

While in choosing Make I'm technically not using a bash script, it's basically the same, just multiple scripts rolled up into one file.

I used it to automate the TF init process, using bash logic to check if the package directory or zip file were already there, and if so, nuke them.  Means I'm starting fresh every time, so if there is a script update, it's definitely getting added into the zip.

So now when I type make apply (as long as I'm authenticated on AWS properly) it'll initialise Terraform, clean up any old Lambda package builds, create a new one, re-downloading the modules, and drop the script in the directory ready for TF to zip it up and ship it off to AWS.

Works a treat!

## Now what?
Well I'm now working on the dynamodb backend.  I'd like the functionality to include the ability to send new things to check pricing on, and using Cron scheduling via CloudWatch Eventbridge to automate checking each hour of every item.  I'll have to figure out the Lambda logic I'm going to use there, whether I bundle up checks into 1 Lambda execution or 1 check per Lambda, which might cause issues if it blows out.

Then I want to tie it into SNS to let me know where these sweet sweet savings can be found!  Let's see how that goes!

I don't NEED a frontend, so if I'm going to worry about that, it'll be at the end/never.

Anyway, rather than a LOTR length snooze fest, I'll split this up into more manageable installments.  Ciao!
