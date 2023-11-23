---
layout: post
title: RSS in your email all through AWS & Terraform
---

![Picture]({{ site.baseurl }}/images/missed_opportunity.png)

I'm a sucker for a sale, and realised that I'd missed one for a subscription provider I use. The timing couldn't be more perfect though, as Black Friday looms on the horizon, I've developed a solution that ensures I—and anyone else who uses it—won't miss out on these once-a-year deals.

It began with a realization that had me kicking myself. A fantastic sale had slipped through my fingers because I hadn't been monitoring the store's RSS feed. In today's fast-paced world, who has the time to constantly check for updates? That's when it dawned on me: I needed a system to bring these deals directly to my phone.

Now I do ask that if you read it, please, I'd like some feedback on how I'm doing the Lambda layer with Make.  Is there a better way to do this?  I'm all ears.  At the bottom is the link!

## Crafting the Solution: Terraform, AWS Lambda, and SNS

![Picture]({{ site.baseurl }}/images/inspiration_while_coding.png)

Leveraging Terraform, AWS and a bit of Python code, I set out to create a practical solution. The plan was to use AWS Lambda for scanning RSS feeds and AWS Simple Notification Service (SNS) for instant notifications.

I developed a Terraform module that was deceptively simple but incredibly effective. It checked RSS feeds for new deals and used SNS to send email alerts directly to my phone. Triggered regularly by AWS EventBridge Scheduler, it was the perfect set-and-forget solution for staying informed.

## The Perfect Timing for Black Friday

![Picture]({{ site.baseurl }}/images/trolley_full_of_toys.png)

With Black Friday just around the corner, this module couldn't have come at a better time. It's not just a personal win but a boon for anyone looking to snag the best deals this shopping season. Every new deal or discount that pops up in the monitored RSS feeds is now instantly pushed to my email, which shows up on my phone. No more missed opportunities.

## Sharing with the Community

![Picture]({{ site.baseurl }}/images/show_off_to_group.png)

As usual with my pokings around, especially with the festive season sales kicking in, I've made the module publicly available on GitHub. It's completely free and ready for anyone to use. Whether for Black Friday sales or any other alerts, this testament to learning while fixing a problem.

As Black Friday 2023 approaches, this little project of mine stands as a beacon for all deal hunters. It's a story of turning a missed opportunity into a resourceful tool, just in time for the biggest sales of the year.

[https://github.com/joebywan/tfmodules/tree/main/rss-email](https://github.com/joebywan/tfmodules/tree/main/rss-email)

This one uses make to build the Lambda layer, if there's a better way to do it, I'd love the feedback please.

Thanks to Dall-e for the pics.  I doubt there's a group of people looking that enthusiastic around a laptop for this (especially sticking their hand through a table!) :D