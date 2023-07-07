---
layout: post
title: CORS is client side!?
---

Today we're diving into the topic of *AWS API Gateway* and *CORS*. For those who may not know, CORS stands for Cross-Origin Resource Sharing. It's a subject that often induces a collective groan, but it's crucial to our web-based world, and there's always more to learn.

I recently embarked on a learning journey where I decided to delve deeper into AWS API Gateway, deploying it using Terraform. It seemed straightforward at the onset, but little did I know, I was heading towards a hurdle.  Everything worked from the API Gateway test functionality in AWS Console, but remotely, nothing worked.

It turned out to be a CORS-related issue. Now, if your memory serves you as it served me, you'd recollect CORS being portrayed as a server-side control in the AWS certification study materials. But, after doing a deep dive, I came to a realisation that shattered my initial understanding of CORS.

![CORS is client side!]({{ site.baseurl }}/images/cors_is_client_side.png)

As it turns out, CORS is more of a client-side control. The server does play a role, but it's more about informing the client-side control, not dictating the entire process. It's a vital distinction and one that we need to keep in mind when we're designing and working with APIs.

This means that the server isn’t the one blocking things, it’s your browser that’s doing it!

So with the understanding that CORS was a client-side control, meaning the browser was the one blocking the requests, I figured that there had to be a way to disable it for testing. Well, there's a useful browser add-on I stumbled upon that can help you with that. It's called CORS Everywhere, and you can find it here: [CORS Everywhere](https://addons.mozilla.org/en-US/firefox/addon/cors-everywhere/).

This extension allows your browser to interact with any server without worrying about CORS, making it a helpful addition to your development toolkit. (Oh yeah, I use Firefox! Come at me Chrome fans :D )

I'm sharing all of this not only to deepen my understanding but also to help others who might be going down a similar path. There's a quote I love by Joseph Joubert, "To teach is to learn twice." By explaining this, I'm reinforcing my understanding, and I'm hopeful that I'm helping you along your learning journey too.

In closing, we need to remember that the world of technology is ever-changing, ever iterating. What holds true today may not be accurate tomorrow, and that's why it's essential to keep learning, investigating, and sharing, just like we've done today with CORS and API Gateway.

Whether you're a newcomer to the tech world or a seasoned professional, challenging your existing knowledge is a great way to grow. So next time you encounter an unexpected problem, like I did with CORS, don't shy away from digging deeper. You might be surprised by what you learn.

By the way, all that said, if I’ve gotten anything wrong, let me know!
