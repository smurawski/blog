---
layout: post
title: "There Is No Configuration Management Special Sauce"
date: 2014-10-22 15:11
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


I get asked quite frequently "Can I do $SomeTask with (DSC|Chef)". &nbsp;My answer is almost always a qualified "yes".


## Two Motivations

<p dir="ltr">When I get this question, very often there are two questions lying in the background. &nbsp;


*   If I roll up my sleeves and dive into it, can I create automation for $SomeTask with (DSC|Chef)?<p dir="ltr">The first question is pretty close to what was asked. &nbsp;The intent of this asker is to determine if there are hidden "gotchas" in automating a task with one of the configuration management platforms or if $TargetSoftware allows $SomeTask to be done in an automated fashion.


*   Is there an existing resource/cookbook/snake oil vial that does $SomeTask with (DSC|Chef)?

In this case, the asker really wants to know if either in-box or some third party has done the work to automate that process that the asker can pick up and use. (There is nothing wrong with looking for existing resources - it's pretty smart actually.)


Both questions are valid. &nbsp;The problem I see with these two questions is very often the asker doesn't necessarily know which of these sub-questions they really intend. &nbsp;This is due to a major lack of familiarity with what configuration management platforms actually do. &nbsp;There is a perception that Chef, DSC, and other configuration management tools offer some enhanced capabilities outside normal automation techniques.


## There is No Magic



DSC, Chef, and other configuration management platforms don't provide any mystical capabilities or imbue systems with otherworldly powers to automate the unautomateable. &nbsp;


I'm probably going to step in a big pile of manure here, but I'll say it. &nbsp;As systems administrators in the Microsoft space, we have been coddled far to long into thinking that Microsoft (and other major vendors) can deliver the administrative workflow that fits all our our organizations. &nbsp;


Don't get me wrong, there are sysadmins in the Microsoft space doing amazingly innovative things. &nbsp;The time, however, has come for us to stop looking to Microsoft (and other major vendors) as the source of **all solutions and workflow**. &nbsp;We need to expand our gaze and look around the industry. &nbsp;Things are changing quickly. &nbsp;Demands on IT are increasing. &nbsp;Business requires more responsiveness from internal IT departments. &nbsp;The risk of failure has more dire consequences with each failure putting you further and further behind, since your competitors are also picking up their cadence.


To its credit, Microsoft has realized this and we are seeing an unprecedented openness in the integration story around Windows Server, Azure, and the development tooling. &nbsp;[Jeffrey Snover](https://twitter.com/jsnover), [in his Monad Manifesto](http://www.jsnover.com/blog/2011/10/01/monad-manifesto/), laid forth a roadmap to make Windows Server automateable, putting the management power and flexibility in the hands of the customer - not the product teams.


Now it is our time, as the customer, to take ownership of our automation needs. &nbsp;No one knows the needs of your organization better than you (and your team). &nbsp;You need the agility to define management workflows for your organization and not have to wait 1 to 3 years for the next version to ship (then another 4 or 5 years for your organization to roll out said upgrade). &nbsp;


Let's stand up and get our hands around what our configuration management platforms can do for us. &nbsp;And then, let's get to building our automation workflows!


## Resources, Providers, and Other Primitives

<p dir="ltr">Most configuration management platforms have the concepts of resources, which are typically abstractions on various system components. &nbsp;Those resources are usually implemented in ways that reflect the target platform. &nbsp;For example, if you look at the Chef "service" resource, there are different implementations for various OS types, since you'd manage a service differently in CentOS vs Windows Server 2008 R2.&nbsp;


For the most part, configuration management systems end up calling command lines or system APIs via whatever language they are implemented in. &nbsp;In order for a configuration management system to actually manage a component, it has to be manageable to begin with.


## Back to the Original Question



This means, the answer to our initial question - "Can I do $SomeTask with (DSC|Chef)?" - is "yes" as long as there is a command line application that can do $SomeTask or if there is an API that your configuration management platform extensibility language can talk to.


If we dig into the deeper question - "Can I implement something that (DSC|Chef) can run to do $SomeTask?" - is totally dependent on $TargetSoftware that supports $SomeTask we are trying to automate. &nbsp;It is&nbsp;


Likewise, the question "Is there something out there that does $SomeTask for (DSC|Chef)?" - becomes very easy to answer. &nbsp;If the supplier of $TargetSoftware doesn't supply DSC Resources or Chef Cookbooks (or other CM tool packages) and there is not a community version that meets those needs, then the answer is "No".


## So What Good is a Configuration Management Platform Then? &nbsp;Wouldn't a Provisioning Script Be Easier?



In both cases, neither DSC nor Chef (nor other configuration management platforms) are what make tasks automateable. &nbsp;They provide a framework for running those actions in an idempotent fashion, where the configuration management agent tests to make sure things are where they should be, and if not can repair the state of the system. &nbsp;This is where configuration management tooling starts to separate itself from provisioning scripts, which don't necessarily adhere to this pattern.


Configuration management tooling also provides an ecosystem of tools and resources for distributing configuration data, testing configurations, managing versions of configuration documents, and provide reporting and analytics.

