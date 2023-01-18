---
layout: post
title: Redoing symlinks
---

## How much do you need for a post?
I've had a couple of people lately ask me how I come up with what I write about.  It's usually something I'm happy with achieving, or something I've been having trouble with, or just something I've talked to people about multiple times, so I find it's easier to write an article so I can tell people to go look at that instead of repeating myself again (and potentially forgetting to tell them something).  So this is a post to show that you don't have to post some grand symphony, just something you're enthusiastic about and want to share.

## So getting to the point!
Had a client who was having issues with linux symlinks breaking during patching.  It'd still show the links as there, but they wouldn't be traversable.  

They requested we do a bunch of manual steps to make sure they're good, and if not, more manual steps to fix them.  Being that it's a repeatable thing, I decided I'd try and automate it.

Now I've been playing with code for enough years in enough languages that I get the logic, but just need to research the syntax for the most part.

Here's the code: [https://github.com/joebywan/fixsymlinks/blob/main/fixsymlinks.sh](https://github.com/joebywan/fixsymlinks/blob/main/fixsymlinks.sh)

Couple of things that tripped me up initially, I figured with the bash if statements having the handy -L which checks for a symbolic link, it'd be fine to just use that.  Figured out that it'd still detect busted links, so I did some searching, and found a handy article online showing that you have to use -L with a nested -e.  The -e attempts to traverse the symlink, checking if the destination is accessible.  So with that sorted, I got to work on the being able to do a whole directory of them.

The awk '{print $9}' is handy in that it lets you print columns of whatever's piped in, so it prints out only the found directories, then the linkdest is the other bit of magic I found online to just capture the symlink destination.  I have to admit sed is still a mystical beast I usually interact with through google searches and stealing code snippets, but I do love it when it works.

## That's it?
Yeah that's it.  As I said, an article doesn't have to be anything grand.  I'm happy with what I managed to cobble together, even if it is only 23 lines, it'll save plenty of hours into the future of other people's time who don't have to do those manual steps.  So yay me!

So if you're worried about posting something, don't be!  Fly your flag!  Toot your horn, champion your successes.

If it was useful to you, great, love to hear about it in the comments on LinkedIn.

![Bash]({{ site.baseurl }}/images/Gnu-bash-logo.svg.png)