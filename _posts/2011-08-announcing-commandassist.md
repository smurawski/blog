---
layout: post
title: "Announcing CommandAssist"
date: 2011-08-04 16:41
author: steven.murawski@gmail.com
comments: true
categories: [CommandAssist, Modules]
tags: []
---


<a href="http://download.usepowershell.com/CommandAssist.zip" target="_blank">CommandAssist</a> is a new module designed to provide some graphical help for people who are new to PowerShell or just not as comfortable on the command line.&#160; <a href="http://download.usepowershell.com/CommandAssist.zip" target="_blank">CommandAssist</a> relies on <a href="http://showui.codeplex.com" target="_blank">ShowUI</a>, so go get it now!



### What’s There Now




The first three functions in this module are:



1.  Show-ParameterAssist
2.  New-AssistedModule
3.  Where-Object (with an updated –Assist parameter)


**Show-ParameterAssist** is the workhorse behind the coolest feature in this module.&#160; Parameter assist takes a command (either a CommandInfo from Get-Command, or a command name and what type of command) and optionally a hashtable of parameters and values.&#160; It provides a UI with all the parameters (grouped in tabs by parameter set).&#160; The UI has parameter help, bolded Mandatory parameters, and let’s you pick enumerations from a drop down menu.



Show-ParameterAssist can be embedded in any function (in the begin block is most effective) before any logic executes.&#160; You can assign the output to $PSBoundParameters and also use it to populate the variables for those parameters.&#160; I’ll publish some examples soon.



**New-AssistedModule** takes a module and creates a new module with proxies for each command.&#160; In these proxies, Show-ParameterAssist is exposed via an –Assist parameter.



The final command (so far) is the most raw.&#160; The module exposes a proxy for **Where-Object**, with an –Assist parameter that provides a scriptblock builder for the filter.&#160; The scriptblock builder has a list of all the properties exposed by incoming objects, as well as a list of all the comparison operators.&#160; You can drag properties and operators onto the textbox to build a scriptblock.&#160; When you have a valid scriptblock, the background turns green.&#160; When it is invalid, the background is red.&#160; Also kind of helpful is a counter showing the number of objects being evaluated.&#160;&#160; When there is a valid scriptblock, there is also a counter saying how many objects will be output.



&#160;



### On the Horizon




For Show-ParameterAssist:



*   More information about the parameters (can it take pipeline input, etc..)
*   If a parameter takes a value by property name, include scriptblock evaluation like in Where-Object
*   Other requests


For Where-Object



*   Offer better information about the incoming properties.
*   Improve the drag and drop experience (and parens, and other commonly used cmdlets and expressions)
*   Improve the help
*   Other requests


For the module in general:



*   More built in assisted commands (like select-object)
*   'Improve the help
*   Other requests     


If you think this module has some value or have a feature you would like to see, please leave a comment or ping me on twitter, or stop by the <a href="http://powerscripting.wordpress.com/2011/08/01/up-next-steven-murawski/" target="_blank">PowerScripting Podcast this evening</a> and ask in the chat room.&#160; <a href="http://download.usepowershell.com/CommandAssist.zip" target="_blank">Grab the scripts</a>, look through them and let me know what you think!

