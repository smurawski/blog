---
layout: post
title: "The Seduction of the Management Abstraction"
date: 2015-05-07 18:30
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


*DISCLAIMER: This is not only about configuring network settings. That is an example.*


## Abstractions are a GOOD THING



Abstractions, in the IT context, are higher level models on top of more complex components.  This allows people who are relatively unfamiliar to quickly gain a minimum level of functionality in new or rarely used areas.


## What are management abstractions?



At their most basic level, management abstractions are the common idioms that we refer to in managing our systems.  One example that comes readily to mind is the network configuration dialog on Windows.
<a href="http://imgur.com/KDTMJKO">![](http://i.imgur.com/KDTMJKO.png "source: imgur.com")</a>

What is actually happening here is that three individual things are happening, two are relatively closely identified to a network adapter, and the other is a general setting.


*   First, we are binding an IP address and subnet mask to a network adapter.
*   Second, we may be assigning a default gateway.  This is somewhat related to the network adapter in that the adapter's IP address should be in the same subnet as the default gateway.
*   Third, we can set DNS servers for the node to use.  This is only related to the network adapter configuration in that they should be reachable via the default gateway.

## The GOOD



Let's focus on the good part of this abstraction - when I first needed to get a machine on the network, I only had one place to go and all the critical elements I might need are right there.


## The BAD



That user interface (UI) ties together three somewhat related elements and, without further reflection or understanding, give you the impression that those elements are treated as a unit.  This was fine, until you needed to drop to a command line to configure something or troubleshot.  "ipconfig" would give you a report similar to the UI you experienced. If you wandered in to netsh land or looked at the route command, you might think you landed on some alien operating environment.


## The UGLY



When Server Core was introduced in Server 2008, the only console available was cmd.exe.  That meant netsh for configurating your IP addresses and route to manipulate your route statements.  Many admins who wanted to experience this trimmed down operating system were hamstrung until a configuration script wrapper was provided.


## The REALLY UGLY



When systems administrators learn only the abstractions and not the underlying technology, a false compentence is felt and implied.  Just because you can set an IP address clicking through network settings does not provide a scalable abililty.  When the same administrator who only understands the network connections UI is asked to provision one hundred (or a thousand) servers (and yeah - DHCP - but this is an example of an abstraction not the only one) and they need their static IPs and network setting configured - he/she is in for a REALLY long, boring task.  


## The Simple is Seductive



It's pretty easy to see that in the case where I might have to do a task several hundred times in a row where diving in and learning to automate that task might be a good thing.  


The challenging part is when those tasks build up slowly.  It may take you a little bit to learn how to script the configuration of network settings and therefore go a bit more slowly on the first couple of times.  The argument of "I'll do it via the UI this one time because it's faster" seems to make sense.  But when you look back a the number of times you've RDP'd (or otherwise console'd) to servers to set network address settings, the potential time savings adds up.


It is not just time savings that are important.  Understanding the structural and behavioral layout of your operating system is immensely useful when it comes time to troubleshoot a problem.  If you don't know how the network stack actually works, troubleshooting network issues can quickly progress past a level where you can be effective.


### Where does that leave you?



#### Are you lost once you leave the sanctity of the management UI?



#### Are you relying solely on the patterns of PowerShell to find the right commands and hoping you have the right parameter mix?



#### Are you searching for a configuration management tool to hide all the ugly complexity from your day to day life?



It's time to dig in and really learn what makes the things you manage tick.  Management abstractions are a great thing for initial adoption and productivity, but when the feces hits the rotating blades a sysadmin needs to understand the components under his/her control.

