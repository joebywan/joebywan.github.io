---
layout: post
title: Pulumi GPT Your IaC Challenge
---

Today I tried Pulumi for the first time.  I was pleasantly surprised with the bells and whistles on it.  A couple that definitely stood out were:

- Natively works with TF state files
- Encrypted Secrets built in!
- Pulumi Cloud Framework (Cloud agnostic modules!) Means you can ask for a bucket and it'll deploy a bucket on AWS, blob on Azure, GCS on GCP.
- Generates code for resource imports!

I've so far been using ChatGPT quite often, as a tool for doing grunt work quickly, but also good for learning new topics.  When I saw their Pulumi "GPT your IaC" Challenge, I figured that's a match made in heaven, lets give it a shot.

The challenge requires the construction of the most complicated thing you can manage by only using Pulumi AI.  There is a command line version, but as I don't have an OpenAI API key subscription, I was using the website version at [https://www.pulumi.com/ai/](https://www.pulumi.com/ai/)

# The Challenge
![https://www.pulumi.com/challenge/ai-architecture/]({{ site.baseurl }}/images/challenge-ai-meta.png)
[https://www.pulumi.com/challenge/ai-architecture/](https://www.pulumi.com/challenge/ai-architecture/)
Before I get into it, I've been really happy with the code for Pulumi and the abilities of it, but I did find the AI experience quite frustrating.

I figured I'd start with:
- EC2 web server
- All that's required for EC2 to access the internet
- Set the test page to "Hello world from Pulumi AI"

I figured I was shooting too low, but for my sanity, I'm glad I aimed here and no higher.

I do pay for ChatGPT plus, which gets me access to GPT4.  I find it worth the expense.  When using the Pulumi AI portal, I think it's great that they are giving unlimited access to OpenAI's GPT4 for generating Pulumi code, but the portal was much slower than ChatGPT Plus and it also disconnected frequently.  As it doesn't support the resumption of conversations, I hope you made a copy before you hit refresh!

I also quite often had it cut off half way through a quite short section of code, and another time I had it generating a response, and it decided to essentially restart generating a response partway through... twice!  So I had this monster length of incomplete gibberish 3 times.

That said I did perservere and got there in the end, it took me about 3-4hrs total.  The code does what I was aiming for.  Initially the AI missed things like the route table attachment to the subnet, starting the web server and dynamically looking up the AMI ID was added after it assumed us-east-1 when I'm working in ap-southeast-2.

Check out my result here: [https://github.com/joebywan/pulumi/tree/main/GPTyourIaCChallenge](https://github.com/joebywan/pulumi/tree/main/GPTyourIaCChallenge)
