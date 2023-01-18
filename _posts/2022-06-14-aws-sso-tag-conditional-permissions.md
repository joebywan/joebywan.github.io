---
layout: post
title: AWS SSO conditional permissions for individual users based on tags
---

I had a client using AWS Cloud9 that had an issue where the users were managing to tax their instances to the point they'd lock up every now and then.

To avoid having tickets for restarting instances, we suggested implementation of stop/start/reboot permissions to the instance.  It needed to be restricted to their particular instance.

We noticed that in creating a Cloud9 instance, it generated a tag called "aws:cloud9:owner":"(string of numbers/letters same for every user):(SSO USER LOGIN EMAIL)"

I initially set them up with a policy of:

{
  "Effect": "Allow",
  "Action": [
    "ec2:StartInstances",
    "ec2:StopInstances",
    "ec2:RebootInstances"
  ],
  "Resource": "*",
  "Condition":{
    "StringEquals":{
      "aws:ResourceTag/owner": "(string of numbers/letters same for every user):${aws:username}"
    }
  }
}

After it didn't work, doing a bit of research, it was found that ${aws:username} returns a friendly name, so just firstname lastname, not the actual login username.

Then ${aws:userid} was brought to my attention, and initially we thought it was going to return the wrong information as it was saying that it would just return the userid of a user.  For IAM users the userid is a unique string of numbers/letters for that user.  We were trying to work with the existing tag, and that wouldn't match.

Ended up having a chat with AWS support who enlightened me in the fact that depending on the source of the user, ${aws:userid} returns different things.

For our use case which was a federated SAML user, it actually returns the SSO role id:caller-specified-role-name.  Digging deeper, with federated users, caller-specified-role-name is actually the username used to connect to the SSO.

So seeing that, we did some more investigation and figured out that the string before the login email in the tag was identical to the role id.

So all I had to do was modify the policy like so:

{
  "Effect": "Allow",
  "Action": [
    "ec2:StartInstances",
    "ec2:StopInstances",
    "ec2:RebootInstances"
  ],
  "Resource": "*",
  "Condition":{
    "StringEquals":{
      "aws:ResourceTag/aws:cloud9:owner": "${aws:userid}"
    }
  }
}

So now that the condition matches the tag on the instance, they can stop/start/reboot.  This could be expanded to other resources to allow users in on a role, but only allow a user to access a certain resource.

For more information on AWS policy variables: https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html