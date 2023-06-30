---
layout: post
title: Automated Least Privilege Implementation for Secure AWS Account Creation
---

In recent times, AWS clients have been more security-conscious, employing Service Control Policies (SCPs) to install guardrails around their accounts. However, there's a pitfall - a new account is by default attached to the `FullAWSAccess` SCP under the root Organizational Unit (OU), allowing it free reign across all of the AWS services.

![Access Denied]({{ site.baseurl }}/images/access_denied_adobe.jpeg)

The Principle of Least Privilege (POLP) is an integral computer security concept where a user is provided with the minimum level of access needed to perform their job functions. POLP is crucial for mitigating unnecessary data exposure risks, potential data breaches, and damage from both internal errors and external attacks. SCPs serve as a practical method to enforce this principle within the AWS ecosystem.

![Least Privilege]({{ site.baseurl }}/images/principle_of_least_privilege.jpeg)

An SCP is a policy type that manages permissions within your AWS organization. The policy outlines the maximum permissions for resource-based or identity-based policies that your organization's accounts can use. However, SCPs' manual management can prove tedious, particularly during the creation of new accounts.

When a new AWS account is formed within an organization, it is attached to the `FullAWSAccess` SCP by default. This policy enables full access to all AWS services and resources, thereby violating the POLP. Consequently, after creating an account, it is common practice to replace the `FullAWSAccess` SCP with a more restrictive policy. But, this step is easy to miss during manual processes.

To address this, we've developed an automated solution employing AWS Lambda, Amazon Eventbridge, and AWS Simple Notification Service (SNS). Amazon Eventbridge is used to detect the successful creation of new AWS accounts. Upon the creation of a new account, Amazon Eventbridge activates an AWS Lambda function. This function checks the new account for the `FullAWSAccess` SCP. If found, the function replaces it with a custom policy. It also sends a notification to an SNS topic to alert administrators about the policy replacement. This process ensures no new AWS account has more access than necessary.

## The advantages of this approach include:

1. **Enforcing the Principle of Least Privilege**: This solution guarantees all new AWS accounts adhere to the POLP from their creation moment, significantly reducing the risk of excessive permissions and potential security issues.

2. **Reducing Manual Effort**: This solution automates a process typically requiring manual effort. Without automation, administrators would need to remember to replace the `FullAWSAccess` SCP each time a new account is created. This solution eliminates this manual step, saving time and reducing human error risks.

3. **Enhancing Security**: By automatically restricting permissions on new accounts, this solution boosts your AWS environment's overall security. If an account is compromised, the damage an attacker could inflict is limited by the custom SCP's allowed permissions.

4. **Ensuring Compliance**: Many organizations have regulatory requirements or security policies that require using the POLP. This solution ensures your AWS environment complies with these policies or requirements.

By adopting the POLP and using automation to manage AWS Service Control Policies, you can create a more secure, efficient, and compliant cloud environment. While it may require some time to initially set up, the long-term benefits of improved security posture, reduced risk, and time savings make it a worthwhile investment. Check out this [link](https://github.com/joebywan/tfmodules/tree/main/auto_Remove_Full_Access_SCP) - I've set it up as a Terraform module that you can employ to automatically stop the full access attachment in your organization.


# The Challenge
![https://www.pulumi.com/challenge/ai-architecture/]({{ site.baseurl }}/images/challenge-ai-meta.png)
