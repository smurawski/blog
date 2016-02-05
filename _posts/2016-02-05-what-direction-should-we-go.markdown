---
layout: post
title: "What Direction Should We Go?"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

I get a lot of questions around Chef, DSC, and SCCM from customers, conference attendees, community members, and others and I'm going try to answer it here.

## Question: Why Should I Use DSC Rather than SCCM

### Snarky Response

Don't.

### More Reasonable Response

It depends.  Unfortunately there isn't a definitive answer as the choice is context sensitive.

If you aren't experiencing any pain with SCCM and you're in touch with your business folks and are meeting their needs in a timely fashion, don't change.

One of the major reasons folks in Microsoft heavy environments start talking about DSC is this desire to build a more "devopsy" workflow.  They keep hearing about how all these other shops are getting faster and faster with their production deployment.  They see talks from [DevOps Enterprise Summit - DOES](https://www.youtube.com/playlist?list=PLGnigCNRJjp5zA02HfuXcr_847VogTYF-) and wonder why it takes them six weeks or more to get a new test environment while their competion is finishing whole projects in that time frame.

*Note:*
If you really want to understand some of the overall business drivers in a technology agnostic way, I'd go read [Leading the Transformation](http://www.amazon.com/Leading-Transformation-Applying-DevOps-Principles-ebook/dp/B012P0D4YG) (and check out the other books on my [DevOps Reading List](http://stevenmurawski.com/devops-reading-list/)).

**If you want your business to care about IT, IT needs to care about the business.**  This is the heart of DevOps.  It's about eliminating business pain and generating business outcomes.
* Why should we deploy faster?  Not because it's technically cool, but so that our business can respond to market demands and try new things more quickly
* Why should we have consistent configuration?  If our ops folks aren't fighting fires would randomly broken servers, they can build more infrastructure to help us deliver faster.  If we want to experiment with Azure or AWS, infrastructure as code let's us quickly build out proof of concepts with our real configurations.
* Why should we automate tests?  Less manual checks are fewer places for inconsistencies.  An automated test will run the same way every time, a tester might skip or combine steps resulting in inconsistent results.  Automated tests can also be run earlier in the process, during the dev cycle.  That frees up expensive human time to more involved testing.

If you are feeling pain or looking for confirmation from other sources that there are better ways, I've got a few technical and cultural reasons.

#### Technical

SCCM is a desktop management tool, first and foremost.  It can be co-opted to manage servers, if you want to manage them like desktops.  Obviously SCCM wasn't meeting the needs for configuration management, or the Windows Server team would not have invested in DSC as a platform capability.

As far as messaging from Microsoft, how much clearer can it be than the guy who invented PowerShell and helped drive DSC now holds the rank of Technical Fellow at Microsoft - one of only a handful of people at that level - and is in charge of architecture for System Center and Windows Server.  Every move in Windows Server is geared towards the movement towards cloud (either for you to run workloads or them to sell services).  What is SCCM's cloud story?  Intune, which is client device management.  The server management story in the cloud is OMS - based around DSC, PowerShell and PowerShell Workflows.

Another consideration is the level of work and experience needed to successfully operate a good SCCM environment.  It's non trivial in the numbers of servers, SQL configuration, AD configuration, and server deployment.  DSC, Chef and others are much lighter weight in their deployment footprint.

#### Cultural

Tools influence culture.  So, if you are striving for a more "devopsy" environment do you want a tool that silo's the process of managing configuration?  Tools like DSC and Chef by themselves don't change culture, but they have their most impact when you adopt certain cultural behaviors. Like:

* Centralizing configuration in source control.
* Having a standard build process that includes automated testing
* Testing and deploying application and configuration together

None of this means you have to relax control of rights or that deployment becomes the wild west.  In fact, these patterns of behavior increase the points of control and move those controls earlier in the process.

You can implement devops practices with SCCM, it's just a tougher path to blaze.

### Wrapping up

At the end of the day, you want tools that support your workflow and YOU need to make that call.  No blogs, articles, talks, books, or other source of information should dictate your tools and workflow.  You (and your business) need to make that call for yourselves.  (Hint: that's why IT Pro stands for IT Professional - it's your professional duty to take the information out there and apply it through your lens of experience and knowledge.)

This means the question isn't "Should I use SCCM or DSC?" but "What Is the Flow of Work Like? And How Can We Improve It?".

If you think you can deliver a better experience for your customers by sticking with SCCM, you should do that.  If SCCM has pain points, you should experiment with and learn about other tools.  Maybe that's DSC or Chef or Puppet or whatever...
