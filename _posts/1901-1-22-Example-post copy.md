---
layout: post
title: SCP != IAM, Least privilege with SCP's & some automation shenanigans
---

# Potential problem

Client AWS environment uses SCP's to slap guardrails on the accounts, to make sure noone's doing something obviously dodgy like trying to request control of a satellite.

We noticed that some of the organisations accounts had the FullAWSAccess SCP either inherited from higher up in the tree, or enabled.

I then read *WAY* too much on SCP's to understand what was going on.

# What's the actual issue

So after reading about it a bit more, I with the assistance of a very patient AWS Support person, I finally wrap my head around it.  Now for context I cut my teeth on Active Directory Users and Groups where inheritance from above grants the permissions.

Where it differs is that SCP's grant the ability to be granted those API permissions.  So inheriting FullAWSAccess from above means that the account can be granted the permissions.  While finding an account with FullAWSAccess means that account can start making brum brum sounds while playing with satellites.

So after getting a better understanding, and digging into the setup, I figured out that the client was creating new accounts, and not removing the FullAWSAccess SCP from the account.

# How to remediate it long term

Now that we'd dug into the root cause being a human factor issue, lets use the paper/scissors/rock equivalent to beat it.  *AUTOMATION!*

Being that it's AWS, we used AWS Cloudwatch Eventbridge to detect when a new account was created, and then used a Lambda function to remove the FullAWSAccess SCP from the account.  This way, we're not relying on a human to do it.

A small note, make sure you're doing your detection in us-east-1.  That's where AWS Organizations posts it's cloudtrail entries for account creation status messages.

The other tricky bit is that when the account's created, it inherits FullAWSAccess from the parent.  So we need to remove it.  Buuuuut AWS stops you removing the last SCP from an account.  So we need to add a new SCP to the account, and then remove the FullAWSAccess SCP from the account.

So... we went and built an SCP that does nothing.

Funnily enough you can put whatever you want in the action and it doesn't check if it's a valid action.  Here's the SCP:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "none:NoSuchActionExists"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    }
  ]
}

Now that issue's resolved, I rolled it all up into a Terraform module you can use [link](https://github.com/joebywan/tfmodules/tree/main/auto_Remove_Full_Access_SCP).  Give it a shot and let me know if you've got any feedback.

Happy avoiding people [sending commands to satellites](https://aws.amazon.com/ground-station/) and costing you squillions :)