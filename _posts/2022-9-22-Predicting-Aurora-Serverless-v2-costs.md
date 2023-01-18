---
layout: post
title: Comparing costs between RDS & Aurora Serverless v2
---

## Comparing apples to oranges?
I had an RDS implementation that the owner wasn't very happy with what they were paying, so I was asked to see how it could be reduced.

Now the standard RDS Reserved Instances were looked into, and would've provided savings, but we looked at the usage of the instance, and it was very bursty, with periods of low usage in between.

## Some background
Before I dig into it, some background on how Aurora Serverless v2 scaling works.  They scale in what AWS calls Aurora Compute Units(ACU).  They consist of 2GiB of Memory, and according to AWS 'appropriate cpu and network bandwidth'.

Considering I was trying to be a bit more exacting, digging around online, people had tested and were finding 1 ACU is equivalent to 0.25vCPU.

Now with that information in hand, we can move on to how I went trying to estimate it.

## You have been weighed, you have been measured, and you have been found wanting
I did some digging around, even spoke to AWS support, and came away disappointed.  All AWS provides is this: [AWS Aurora Serverless v2 calculator](https://calculator.aws/#/addService/AuroraPostgreSQL).

Now if you didn't click the link, they ask how many ACU's you require, and for how long with a per time period (hour, day, month) selector, which proudly does ACUs x time period in hours x per ACU cost.

Now for a service that is supposed to be scaling as often as every 10 seconds, giving a static per hour flat usage cost is the opposite of what Aurora Serverless is trying to achieve.

## One door closes, another opens
So bereft I tried to find something that'd compare apples to apples, but in vain.  If you're familiar with AWS compute optimizer, they use your existing EC2 metrics to compare performance and make right-sizing recommendations from it.

I figured that doing the same for RDS has to be possible right?... right!

## Putting everything together
I built a script that pulls data from cloudwatch, and formats it to output to a CSV file.  Being a data novice, I do my visualisation in sheets.  Don't ridicule me too much :D

* Sheets link here: [Sheets link](https://docs.google.com/spreadsheets/d/1CkLUkyx_AsWRKSOjdEh8W-zEn_SyxP2Wi22zjpvvuGE/edit?usp=sharing)
* Github repo here: [https://github.com/joebywan/cloudwatchRDSCPUtoCSV](https://github.com/joebywan/cloudwatchRDSCPUtoCSV)

Make a copy of the sheet, clone the repo and you should be good to configure the script for your RDS instance.

In the sheet, it should be self explanatory, set your current instance size & specs, then you're good to import the data.  It'll even give you shiny charts you can show off to your family!

Feel free to hit me up on [the LinkedIn Article](https://www.linkedin.com/pulse/comparing-costs-between-rds-aurora-serverless-v2-joseph-howe) if you've got any questions, I post these on there aswell.

![AWS]({{ site.baseurl }}/images/auroraserverlessv2.jpg)