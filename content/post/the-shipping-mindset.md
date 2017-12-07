---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2016-06-10T00:00:00Z
tags: []
title: The Shipping Mindset

---

## TL;DR

In traditional sysadmin work, [we tend to find that whatever is implemented sticks around for a long time](http://stevenmurawski.com/powershell/2014/01/its-just-temporary/).  This can lead to stressing about exactly how to implement a particular thing.  

What I've learned working at Chef is if you focus on shipping and iterating, you can start with a less than optimal solution (but one that gets the job done), and **as long as you are committed to shipping** changing it iteratively becomes trivial.

## Shipping Feels Good
When I changed my mindset from "I need x number of changes before I cut a release" to "make a change and ship it", I began to feel more satisfaction.  When a PR of mine gets merged to master on Chef, I know it'll be in the next nightly build and it'll be out for the rest of the world in a month or less.  When I merge a PR to a community cookbook and ship it to the Supermarket 10 minutes later, I feel good for helping provide a better experience to the consumers of the cookbook and the PR contributor feels good that their contribution is merged and shipped!

## Overcoming Resistance

When I apply this to projects internal to someone's environment, the major hurdle I come across is the feeling of "I want to do this right, the first time" and there is agonizing and handwringing over how we should do things.

When I talk to customers or community members who want to implement Chef or DSC, one of the first things we talk about is setting up a pipeline to deliver change ([Michael Greene and I wrote a little paper on that topic](http://aka.ms/thereleasepipelinemodelpdf)).  

At this point, the wish lists and agonizing start.

* We don't have automated testing.
* We'll need 17 levels of approvals
* Dashboards!
* How can we test this environment? Nobody knows what's there!
* But what about when I just need to make a change in production?

and on and on...

We need to take a step back and start with "Let's get everyone sticking all their scripts and config files in source control."  How can we do that?  With a build process!  Make source control and a build server the gateway to getting scripts and text files into the production environment.  No tests, just a straight pass through which provides the base line for getting things into source.  

After your team gets used to that, you can start adding checks - like [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) to check your PowerShell scripts or [Foodcritic](http://www.foodcritic.io/) to check your recipes in cookbooks.  Since you have your "pipeline", you have that chokepoint to put those new "policies" in place.  You can measure their impact and try different things.  You need that place to start.  It doesn't have to be perfect, but it does have to exist.
