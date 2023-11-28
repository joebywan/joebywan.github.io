---
layout: post
title: AI's Infinity Gauntlet: Sculpting D&D Adventures in a Snap
---

![budget]({{ site.baseurl }}/images/budget_infinity_gauntlet.png)

Your party of adventurers are due to arrive any minute now, ready and eager to slay the undead hordes, save the princess, get that sweet loot and be regarded as the most badass in all the kingdom.

Unfortunately your brain's rolled the dice and it's come up a 1.  You've got nothing.  The creative juices refuse to flow.

Thankfully AWS PartyRock was released recently, and you can use it to create a D&D adventure in a snap!  (See what I did there?)

## What is AWS PartyRock?

AWS PartyRock leverages AWS's new Bedrock service to let you speak in natural language what you want, and it'll chain together some LLM generation in what they call widgets.  The cool ability this has over just using multiple ChatGPT tabs is that you can reference widgets from other widgets to use as context!

## What is this article about?

I get blocked rather easily creativity-wise when it comes to thinking of things for my D&D games.  So, I figured I'd make an Adventure Generator, where the user can input some context, flavour, requirements etc.  It'll reference that in subsequent results, using them to ensure it's sticking to the same theme or requirement you place on it.

## The AWS PartyRock experience

![prepare]({{ site.baseurl }}/images/adventurers_prepare.png)

It was good.  Very easy and intuitive.  It's definitely designed for a super low bar of entry, so anyone can get their hands dirty playing with AI.

When you head over to [https://partyrock.aws](https://partyrock.aws), make an account/login, and you can click "Build your own App".  You're presented with a plain white text box, asking you to describe your desired application.  

Take your time, be as detailed as you like, but keep in mind you're just going to be dealing with text boxes that produce text using AI, or pictures.  You're not designing a user interface(UI), or other functinoality external to the aforementioned boxes/widgets.

Once you're done, click "Generate App".  It'll take a few minutes to generate your app, and then you'll be presented with the app consisting of a number of widgets as it's interpreted your instructions to be.

Try clicking on the edit button for a widget and it'll let you play with the title, prompt and a couple of other settings.  I spent most of my time fine-tuning the prompts, and linking together the widgets in a logical fashion to ensure context continuity throughout the generated adventure.  It took me about 2 hours.

I do have to say though that after feeding it the initial prompt to make the 'app', it did require some fine tuning.  This didn't sour the experience, while I did have to be very specific about certain things, it did make me think more about my intended goal, help me decide what's required, what's not, and refine it down to what exactly I wanted it to show.

## Use case for this?

While AWS is talking about charging for this, I don't really feel like it's much more than a novelty, as it's not presenting a way to use these widgets elsewhere via an api, so you're stuck with their site, their UI, their workflow.  I think AWS should really be using it as a gateway drug for AWS Bedrock, the AI as a service offering they've based Partyrock on.

## Final thoughts

AWS PartyRock represents a leap forward in AI-driven app development:

The good:

- Offering new no code possibilities in transforming ideas into tangible applications
- Ensuring it's a **VERY** low bar so anyone who's curious can play with it

My concerns:

- The pricing of this will be crucial as to whether it's just a novelty that gets forgotten about, or something that's used to test out LLM concepts.
- Currently because it's so feature restricted, it's more a sandbox.  To use the inputs/outputs elsewhere, an API for each PartyRock that could be sat behind cognito for user authentication to offer subscription services would be good. I know you could do it with Bedrock, but this would just mean an easier way to implement, and give AWS the opportunity to charge more

With its vast potential, AWS PartyRock could play a pivotal role in shaping the future of digital innovation in the cloud computing ecosystem.

As is standard with my articles, here's the 'app' I made: ![https://partyrock.aws/u/joehcmd/yV-5X2KxX5/TTRPG-Adventure-Generator](https://partyrock.aws/u/joehcmd/yV-5X2KxX5/TTRPG-Adventure-Generator)
