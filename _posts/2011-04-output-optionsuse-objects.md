---
layout: post
title: "Output Options–Use Objects!"
date: 2011-04-06 08:33
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames]
---


One area that many people get hung up on in creating a PowerShell script is in the output area.  (I’ve been guilty of this too, but I’m trying..)


## One Tool to Rule Them All




PowerShell embraces the "many small tools composed together to create solutions” style of automation, rather than the monolithic “one tool to rule them all (and the command line switches to prove it..)”



What this means is that your script does not have to contain every single export option imagined.  Your script should output objects, which most of the other “tools” in PowerShell are designed to work with.



## Many Tools in the Toolbox




If your script outputs objects, then you have a dazzling array of options.  You can let the shell decide how to display it (there are some built in rules based on number of properties, size of data, etc..).  You can use one of the Format-* cmdlets and specify how you want the output on the screen to appear.  You can use the Select-Object cmdlet to pick and choose the properties you are interested in.  With both the Format-* cmdlets and Select-Object, you can get more fancy and use a calculated property to transform your output.



If displaying to the screen is not your desire, you can use the Export-* cmdlets to export to your choice of XML or CSV. Out-File, Set-Content, and the output redirectors also help you get content into a file.



If a file and the console are too mundane.. throw the results into a data grid with Out-Gridview.



Other modules or snapins can provide you some other export and formatting options (like the cool PowerBoots and WPK stuff), but you greatly limit the reusability if you try to handle all the output options on your own.



## PowerShell is Pluggable




Since PowerShell is so easy to extend, your option options are as limitless as your (and the community’s) imagination, talent, and determination.  Keeping your output as objects keeps your options open.



## This Doesn't Apply to Me




If you are thinking, "Hey, fine, if you are going to be so authoritarian on scripts.. I'll just write functions (or cmdlets)." You are sadly mistaken.  This all applies to functions and cmdlets as well.  (Scripts, functions, and cmdlets all operate similarly and should follow this basic advice.  Send your output as objects and let the user decide how they want to format, serialize, display, or otherwise manipulate it. (Thanks to <a href="http://powershellstation.com" target="_blank">Mike Shepard</a> for reminding me about that.)

