---
layout: post
title: "DevOps and Agile"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

## Agile Vs. DevOps

Agile and DevOps espouse some common ideas and ideals, which sometimes leads to comments like, "Why do we need to talk about this DevOps thing? Aren't we already Agile?"

Some of this is answered by the size of your organization and the level of specialization of roles.

#### What is Agile?

Agile ([per the Agile Manifesto](http://agilemanifesto.org/)) purports to improve software development by favoring:

> Individuals and interactions over processes and tools

> Working software over comprehensive documentation

> Customer collaboration over contract negotiation

> Responding to change over following a plan

Businesses tend to buy into Agile (at least in name) to increase their ability to deliver software better than they were before and be able to respond more quickly to changing business needs.

#### What is DevOps?

DevOps [(per Adam Jacob, founder and CTO of Chef Software)](https://github.com/chef/devops-kungfu#what-is-devops), is a cultural and professional movement focused on how we build and operate high velocity organizations, born from the experiences of its practitioners. ([Adam gave a great talk on how Chef practices DevOps.](https://www.youtube.com/watch?v=_DEToXsgrPc))


## What's the difference?

Both Agile and DevOps are about accelerating the development of software, but DevOps includes a broader expanse of roles and explicity deals with the operation of said software.

### In the Small Business / Startup Space

There isn't much difference between Agile and DevOps in the startup, as you don't see the level of separation of duties.  Developers in the startup space are incentivized to build software that is managable and deployable, as they are often the first line of suppport for it (or the quick second call).

### In the Mid-sized to Enterprise Space

In the mid-sized to enterprise space, we tend to see people specializing into more dedicated roles.

In this space, when we have Agile practices cranking up developer productivity and "releasing" software more frequently

![Devs shipping like a conveyor belt](https://stevenmurawski.com/talks/DevOps-Images/conveyor_belt.gif "Devs Be Like")

and IT operations isn't part of the conversation,

![Ops still deploying one package at a time](https://stevenmurawski.com/talks/DevOps-Images/ops_deploy_package.gif "Ops Be Like")

we end up with a pile of "finished" software that doesn't do the business much good.  Those "releases" may have been great for QA to test, but most will never see the light of day and live on some backup tape stored offsite for posterity.



#### This is where we need DevOps.

**Bi-weekly software releases + quarterly software deployments <> velocity**

We can make our dev teams stronger, faster, and get them delivering software more reliabily.

Unless we address our ability to stretch that software development pipeline all the way to operations, Agile cannot be fully realized.

**This Is The Difference Between Continuous Integration and Continuous Delivery.**

IT Operations isn't the only part of the business that needs to level up though.

As roles were specialized, developers became further separated from actually running their software in production.  Without real and painful feedback, developers lost the incentive to deliver operations-ready software.

Once we start extending the delivery of that Agile release into production more quickly, it becomes harder to hide gaps in management and instrumentation.

* When servers become an artifact that we stamp out on demand, rather than the lovingly crafted monstrosities that they are today, it becomes harder shift blame around as to what is causing the failure.
* When IT operations becomes part of the delivery pipeline, delivering production-like servers to test environments becomes realistic.
* When developers test their software in a production-like environment, misconfiguration and unexpected behavior incidents decrease and stability is improved.

DevOps is the realization of Agile in organizations where developers aren't the ones running the software as well. 