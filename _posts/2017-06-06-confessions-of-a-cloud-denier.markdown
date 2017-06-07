---
layout: post
title: "Confessions of a Cloud Denier"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

I've been pretty cloud agnostic (as in not believing in cloud, not necessarily any particular provider) to cloud hostile over the past few years.  I've been supportive of cloud efforts of friends and co-workers, but I saw it as glorified virtualization with less control and worse performance than local workloads.

My last few jobs before Chef were very dependent on local machine performance and data gravity (keeping the apps close to the data).  Cloud just didn't make sense without a massive re-architecture for which there was no driving business need.

At Chef, almost everything I worked on was OS and up, so where that OS ran was irrelevent.  I used a bit of cloud for testing, but I kept coming back to local machines for speed.

## But Steve, aren't you going to work as a Cloud Developer Advocate?

Yep.  All will be explained shortly.

### What has changed?

I had to step back and look at why I do what I do and why I want to do what I do.

When I left Stack Overflow, which is arguably one of the best jobs for a Windows IT ops person, I chose to leave because I wanted to share what I thought was a valuable message to a broader audience.  I wanted to help raise the bar for how IT ops works for Windows infrastructure by introducing configuration management.  I was hooked after working with Puppet on Linux and DSC on Windows.  Chef was very open to integrating with DSC and with the Windows admin community, so I went there.

But IT is changing.  While configuration management is still a worthwhile thing, there are other moving parts that are changing the way that we operate systems.

Cloud just isn't a way to run virtual machines anymore.  Infrastructure as a Service (IaaS) is table stakes and where most IT ops begin diving in.  But the real value is all the other services in the cloud.  Services are the game changer.

### We've had hosted services forever though

While we've had hosted services computers have been a thing, the increased availability of internet access (which enables more hybrid scenarios) and the large service providers (e.g. Google, Amazon, Microsoft) who have hundreds and thousands of developers building infrastructure for the same services you need to provide internally have tilted the balance of power.

The services that the cloud providers offer are published with prices, SLAs, and can be spun up in minimal time.  Now, the IT organization has a minimum level of service that they have to beat for those base services.  This isn't a bid from a small managed service provider with a limited interface for management and control.  These are larger providers with APIs that allow customers the control that they need.

### What hasn't changed?

Developers and IT operations are still on the hook for delivering solutions that meet the needs of the business.

### You still haven't sold me on why I need to know how to evaluate and use cloud services.

If we can consume a service and cut out a large part of the standard care and feeding (for example, rather than own your own internal TFS instance - using the hosted service), we gain back that time and personnel to focus them on things that matter to the business outcome.  Unless you are in the business of hosting TFS for example, running your own TFS instance is just overhead.  While it enables a lot of work to happen, you can compare your costs to run it directly to the costs of the hosted service.  Whether you run it better or more efficiently only matters for a period of time.

Do you, as an IT ops person or developer, want to be in a place where you are competing on price against the scale of cloud services?

Or, do you want to differentiate yourself and focus on outcomes that enable and support your business?

You cannot ignore what is happening in the cloud.  While executives and the C-suite may have not really be able to put real dollars around the value being provided by their IT organizations, the abundance and variety of cloud providers (and their cost competitiveness) is putting a lot of competitive information out there.

You can hide and say, "We can't use the public cloud" and that may be true for a while, but do you want to bet your career on that?

And you can start getting ready to justify why when you run SharePoint, it costs your organization significantly when they already pay for O365...

AzureStack is just around the corner, so virtualization and infrastructure admins be ready...  If you don't have enough automation in place to compete, AzureStack is there with a consumption model for pricing - so they have an incentive to make it easy to spin up infrastructure and services.

## Ready or not...

The cloud is coming.  Which direction will you take your career?
