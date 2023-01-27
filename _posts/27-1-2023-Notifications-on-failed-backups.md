---
layout: post
title: Notifications on failed backups
---

# Who cares about failed backups?

In the past, all too often I've worked somewhere, or interacted with an external client that are managing backups.  When I ask how they're monitoring them, or atleast receiving notifications on failures, I get looked at like I'm crazy.

![but_why?]({{ site.baseurl }}/images/jackie_why.jpg)

To me, from my time in on premise IT, those were compulsory so you could re-run a backup or investigate why it failed. Just having those notifications saved our bacon with clients multiple times.

I know that AWS is super reliable, and I've not seen a time a backup has failed in my time working on AWS.  Call me superstitious, but I'm a firm believer in this summary of Murphy's Law. "Anything that can go wrong, will -- at the worst possible moment.".

# Be the change you want to see
So with that in mind I figured instead of screaming into the void, or railing at others for not doing it, I'll be the change I want to see and provide people with tools to achieve it themselves.

I've built a cloudformation template that deploys:
 - Backup Vault
 - 3 x plans, every 7 days, 14 or 28 days
 - backup selection criteria to do it based on tags. Use key "backup" and value 7, 14 or 28 to use the respective plans.
 - SNS topic to subscribe an email address
I also constructed it as a terraform module, which also allows the supply of an array of days apart that each plan should execute, and it'll create them dynamically.

They're available on github here:
- Cloudformation: [https://github.com/joebywan/AWS_coding/tree/main/aws_backup](https://github.com/joebywan/AWS_coding/tree/main/aws_backup)
- Terraform: [https://github.com/joebywan/tfmodules/tree/main/aws_backup](https://github.com/joebywan/tfmodules/tree/main/aws_backup)

So you, yes you!  You have no excuse for not having backup notifications on your AWS infrastructure.
