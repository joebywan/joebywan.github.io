---
layout: post
title: Terraform instance scheduler on the cheap
---

In the realm of cloud computing, managing resources in a cost-effective manner is a critical concern for many organizations. A popular cost-saving strategy is to schedule the start and stop times of instances based on the needs of the workload. While AWS provides a solution with its AWS Instance Scheduler, it comes at a monetary cost and is built on AWS SAM & CloudFormation, which may not align with the tech stacks of all organizations. Recognizing this gap, I embarked on a journey to create a Terraform-native solution that not only aligns with the technology preferences of Terraform aficionados but also presents a cost-effective alternative.

The fruit of this endeavor is a Terraform module I built that you can use! [https://github.com/joebywan/tfmodules/tree/main/instance_scheduler](https://github.com/joebywan/tfmodules/tree/main/instance_scheduler) This module was born out of a desire for a Terraform-centric approach as opposed to the AWS-centric approach embodied by AWS Instance Scheduler. The module I built seamlessly integrates with your existing Terraform workflows, allowing for straightforward scheduling of instances, all while keeping the operations nestled within the familiar confines of Terraform.

Cost optimization was a major driving force behind this project. Unlike the AWS Instance Scheduler, which comes with a price tag of approximately $5 per month, the terraform instance scheduler shines with up to 14 million executions free per month under the AWS free tier. This significant cost-saving potential makes it an attractive solution for organizations looking to trim down operational costs.

Diving a bit into how 'tfmodules' operates: It uses Eventbridge scheduler which allows us to use the AWS API itself as a target, so no intermediate Lambda functions.  This is all scheduled by cron expression, and providing it a list of the instances to schedule.

It's set to tag the instances after scheduling just to make it a bit more obvious that they're not just randomly turning off, they're power scheduled.