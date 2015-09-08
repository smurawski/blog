---
layout: post
title: "PowerShell V3 – Auto-loading of Modules"
date: 2012-01-06 15:32
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 3, PowerShell Version 3 CTP 1, PowerShell Version 3 CTP 2]
tags: []
---


PowerShell V3 includes a new “feature” – Auto-loading of modules.


### What Does It Do?



Module discovery has been updated to make the exported commands for modules that are not loaded visible in a PowerShell session.


In Version 2, if you run Get-Module on a module that is not loaded but in your PSModulePath, you’ll get some metadata about the module, but nothing about the ExportedFunctions, ExportedCmdlets, ExportedAliases, or ExportedVariables.&nbsp;


Version 3 changes this, allowing Get-Module to retrieve information on all the exported features of the module.


This enables tab completion to complete the names of commands from modules that are not loaded.&nbsp; It also provides information to the PowerShell runtime on where to get the command information from, if the module is not loaded.


So, in Version 3, if you run a command from a module that is not loaded, the runtime will search your PSModulePath for the first command that it can find that matches that name and load the module that command is in for you.


### I’m Not Sold



I really don’t care for this feature.&nbsp; I like to be in control of what module I import.&nbsp; The order in which modules are automatically imported depends on your PSModulePath.&nbsp; You don’t get to pick what the order is…&nbsp; User specific PSModulePath entries are first, then machine specific PSModulePath entries.&nbsp; This can cause a host of unexpected problems when there are command name collisions.


For example, the new Hyper-V cmdlets for Windows 8 cannot manage down-level Hyper-V machines, so you might need [James O’Neil’s Hyper-V module from Codeplex](http://pshyperv.codeplex.com/).&nbsp; Both modules have Get-VM.&nbsp; Since James’ module will be in my user PSModulePath (most likely), unless I specifically load the Windows 8 Hyper-V module, running Get-VM will always load James’ module.


 


**UPDATE:** There is a preference variable to control this. &nbsp;$PSModuleAutoLoadingPreference. &nbsp;The possible values are


*   "All" - which will import the module of a command on first use,
*   "ModuleQualified" - which will import a module when you specify module name\command name, and
*   "None" - which disables module autoloading.
