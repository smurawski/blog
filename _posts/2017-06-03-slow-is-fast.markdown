---
layout: post
title: "Slow is Smooth. Smooth is Fast"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

The 2017 PowerShell/DevOps Summit in April was a great event.  I spent a lot of time talking to folks about different workflows and how to approach building out their pipelines to deliver their configuration management.

One of the central oppositional themes I heard was, "We don't need to deploy 15 times a day, so why go through the extra effort?"

## Re-Thinking How We Deliver Change

Regardless of how often you deploy software (and into what number of environments), you very likely don't change your environment on the schedule you think you do.

There are a number of activities that change the state of your environment that are either deemed "exceptional" or "insignificant".  

### You keep using these words, I do not think it means what you think it means

#### "Exceptional"

Almost everyone I've talked to (including myself - yes, I talk to myself) has experienced a deployment that didn't go as planned.  Extra logging is enabled in prod, app pools are recycled, config settings are twiddled, and emergency patches are crafted.  This is almost never logged and never acknowleged when planning deployment and maintenance windows (and often can violates our SLAs).

#### "Insignificant"

OS patches roll out periodically.  Event logs fill up and need to be cleared.  A new security policy is applied via GPO.

#### What if...

While we may not have acknowledged that these changes happen, they are part of the lifecycle of the infrastructure we manage.  

* What if these changes were all done in a standard way?  
* What if there was tracking (history) of all changes (including emergency changes)?
* What if we had centralized event logging and all changes to enable new logs were delivered via configuration management?  
* What if emergency patches were rolled out with the push of a button or commit to source control?

These are all possible and become easier when we build a pipeline to standardize the delivery of change into our environment.  We then have a place where we can add tests (to increase safety and minimize risk in our change).  We reduce the number of places people have to intervene (and thus places that can vary and cause future problems).  

While we may not need to deploy 15 times a day, we do need to deliver change into our environments in a way that is consistent, resilient, and repeatable.  There was a phrase my instructors used in my firearms and defense and arrest tactics classes - **slow is smooth and smooth is fast**.

### Slow is Smooth

**Slow is smooth** (in the context of building an infrastructure delivery pipeline) means that as we start to build this capability iteratively.  We start with being able to deliver one change to one server.  Maybe it's just deploying a file or setting the time zone.  We validate that we can check something into source control and *the right thing* happens on the server under management.  Then we add another server, or set of servers.  We make sure the same change can be delivered and is applied correctly.  We build confidence in the process.  We start to add more elements of the configuration to our model for the server and deliver that to the subset of systems under control.  As our confidence and scope grows, change becomes delivered much more smoothly.  We start to develop tests to catch the things that went wrong.  Even better, we start to learn what "going right" looks like and build tests to make sure that before we even change  the systems we have a high degree of confidence that the change will succeed.

### Smooth is Fast

As our changes go more smoothly, we start to realize that **smooth is fast**.  We've built the capability to deliver change on demand (with whatever lag the pipeline tests add).  We now know how long it takes to get change into the environment in a consistent, auditable, repeatable way.  While we may not need to deploy 15 (or any other number) of times a day, we know that when we do have to deliver a change to our environments, we can.  We are no longer the bottleneck or the blocker.

Building a pipeline to deliver change isn't about deploying constantly.  It is about having the capability to deploy change when you have the business or operational need to do so.