---
layout: post
title: Terraform Instance Scheduler v2, now with Tags!
---

So after releasing the first version, I had a workmate point out that it might be possible to do something with Group Lifecycle Events which had only just been released in March this year.  I had a look at the documentation and it looked like it was possible, so I decided to give it a go.  I'm pleased to say that it worked!  Bear with me, I'll give you something to play with after the writeup.

## The Problem

![Problem]({{ site.baseurl }}/images/error.png)

AWS Scheduler costs money.  I don't think a scheduler should cost, should either be provided by AWS, or free!  So I wanted to build one that fit solely within free tier, or was free.

## The Solution

![Solution]({{ site.baseurl }}/images/solution.png)

Eventbridge Scheduler does the heavy lifting, being able to trigger the Start/StopEC2Instances API call directly.  Then to make it so we can apply tags to enable power scheduling just like AWS's solution, a Resource Group's created for just EC2 Instances, and the tag specified when you deploy it.

Now the hidden magic that makes it work is AWS recently introduced Group Lifecycle Events which lets Eventbridge trigger from Resource Group changes.

So we can set an Eventbridge rule to trigger when an Instance either joins or leaves the Resource Group.  It then updates the Eventbridge Scheduler's list of instance id's to target when the cron schedule triggers.

## Lets crunch some numbers...
![crunch_numbers]({{ site.baseurl }}/images/crunch_numbers.png)
A little reminder on free tier:

* Lambda provides 1,000,000 executions or 400,000GB-seconds per month for free.
* Eventbridge Scheduler provides 14,000,000 executions per month for free.

Each Lambda execution is approximately 500ms @ 128MB, meaning that your only limit is the 1,000,000 executions per month.  Good luck updating that many instances :)

Then Eventbridge Scheduler, it's able to take an infinite number of instances per execution.  So if you're running a power on and power off schedule on each weekday, you're looking at about 40-50 executions per month.

## The Code
![Code]({{ site.baseurl }}/images/code.png)
So thanks for listening, I've got the module, docs and an example to deploy it up in my tfmodules github repo.  Give it a try, I'd love feedback! 
[https://github.com/joebywan/tfmodules/blob/main/free_tags_instance_scheduler/README.md](https://github.com/joebywan/tfmodules/blob/main/free_tags_instance_scheduler/README.md)


![Picture_Example]({{ site.baseurl }}/images/resume.png)

- List item 1
- List item 2
    1. Sub numbered item 1
    1. Sub numbered item 2

![Job Hunt]({{ site.baseurl }}/images/job.jpg)