---
layout: post
title: "MMS 2013 - Advanced Automation session"
date: 2013-04-15 22:12
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


I had a great time [so here they are...](/s/DemoScripts.zip)


## The Demo



While the core of our session focused on the scenarios that PowerShell V3 enables, our principal demo was around configuration management. &nbsp;We started with a running domain controller and a base VHD. &nbsp;From there, we provisioned two web servers with a call to Assert-VM. &nbsp;When the two web servers were ready, we deployed our website with Assert-Website. &nbsp;


## Behind the Scenes



The scenario&nbsp;highlights the power that&nbsp;declarative&nbsp;configuration management holds. &nbsp;Providing configuration variables in script files leaves an artifact that can be version controlled, providing a safety net for changes and allowing all versions of the environment to be replicated. 


By breaking the configuration into an environmental and a structural&nbsp;component, we isolate where changes need to be made, minimizing the places data needs to be changed, and as we learned early in the presentation, changes are a major driver of failures.


The second major concept demonstrated the is&nbsp;idempotent operations. &nbsp;Idempotent operations mean that the command can run time and time again, and the only result will be the direct effect of the command, with no additional side effects. &nbsp;Writing functions to act in an&nbsp;idempotent&nbsp;manner&nbsp;allow us the freedom to run the command as often as we would like without fear that our production environment would be impacted. &nbsp;This allows us to make changes to some configuration items and re-run the whole configuration script. &nbsp;Our existing configuration would be validated and the changes then applied.


If you are responsible for server configuration, it'd be to your benefit to check into configuration management systems like this (e.g. Chef, Puppet, and CFEngine amongst others) and how you can leverage these techniques in your build and deploy process.

