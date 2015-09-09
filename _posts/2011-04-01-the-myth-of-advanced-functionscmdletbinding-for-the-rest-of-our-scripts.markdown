---
layout: post
title: "The Myth of Advanced Functions–CmdletBinding for the Rest of our Scripts"
date: 2011-04-13 14:50
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames, parameters]
---


It’s been killing me to see scripts that are written like V1 functions and having these wonderfully written V2 style advanced functions contained inside of them.



I would like to share a secret about the CmdletBinding, Parameter attribution, and the rest of the capabilities of Advanced Functions.



**These features are not just for Functions!**



## Those Things That Keep Us Apart, Bring Us Together




There are a couple of key differences between functions and scripts.



1.  A function is declared at runtime and kept in memory. 
2.  A script resides in a file on the file system. 
3.  A function is declared with      


function Add-SomeNameHere


    
4.  A script is declared with a file extension of .ps1. 


**The file name (before the .ps1) replaces the “function YourFunctionNameHere”.**&#160; 



**The file itself acts like curly braces surrounding the scriptblock contained in the file.**



## Use the Pipeline Luke!




*Sorry for the bad Star Wars reference..*



Since we’ve shown that scripts and functions share more in common than they don’t, we can build our scripts to take advantage of the pipeline.&#160; 



I can start them out with [CmdletBinding()] and add all the ShouldProcess goodness, or use set a default parameter set.



I can set up Begin, Process, and End blocks in my script, just like in an Advanced Function.



I can use the [Parameter] attribute and specify parameters that can come from the pipeline, make them mandatory, and all the other goodness I could do with an advanced function.&#160; 



And of course, I can add all the Comment Based Help I can think of.

