---
layout: post
title: "Where to Filter"
date: 2011-04-11 06:00
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames]
---


## Where-Object Rocks!




PowerShell has some great filtering capabilities.&#160; Where-Object is extremely versatile, allowing you to filter on any logic you can come up with.



##  I Feel a &quot;But&quot; Coming Up




However, many cmdlets, functions, or scripts have some built in filtering capabilities.&#160; These are often optimized for the command, and if the command supports remote connections, often the filtering can happen on the remote host.&#160; Where-Object is the least common denominator in filtering.



## Use the Help




There are way to many commands and parameters to be familiar with them all.&#160; I spend a good bit of time reviewing the built in help to see the parameters that are taken, whether they allow wildcards, and seeing what the examples are recommending.&#160; Over time, you'll develop a comfort level with certain commands and their options, but there are always going to be new ones to explore. 



## But What If I'm Only Working at the Command Line?




When I'm working at the command line, I'll be more likely to use where-object, because speed in creating the command line is often more important than squeezing every last bit of performance or executing with perfect form.&#160; When I write a script though, something that will be reusable, I try to take full advantage of what the commands offer.

