---
layout: post
title: "Tip: Free Up Some Memory"
date: 2009-03-25 19:03
author: steven.murawski@gmail.com
comments: true
categories: [-NET Framework, General, PowerShell, Tip]
tags: []
---


I’m often running too many applications on my workstation, and at least one session of&#160; <a href="http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx" target="_blank">PowerShell</a> is almost always open.&#160; I’m regularly jumping back to a PowerShell prompt to try something out, query something from my database, or complete a task.&#160; 



If I don’t clear out the variables that reference objects I’m done with, the amount of memory used by my PowerShell session increases dramatically, which impacts some other applications that I run.&#160; (I know, memory is cheap.. buy more memory.&#160; I’ve got a limited budget and I have to prioritize.)



So I’ve written a couple of functions that can help me clear out the extraneous variables and free up some of that memory being used by PowerShell.



The first function (and I’m open to suggestions on the naming here..), [saves them to the AppDomain](/blog/2009/03/tip-sneaky-storage-whats-in-your-appdomain) (so I don’t lose them when I clear out variables).&#160; I recommend running this shortly after starting PowerShell or running it from your profile. 



Later, when I’m feeling some memory pressure, I can run the second function <a href="http://poshcode.org/973" target="_blank">Remove-NewVariable</a>, which calls Remove-Variable for all variables whose names I did not save in the AppDomain.&#160; This removes the references to the objects they represented.&#160; 



**Info Overload: Notice that I said that it removed the references to those objects.&#160; Those objects still exist in memory and remain there until the CLR “garbage collects” them, releasing that memory.&#160; The CLR has a process to reclaim that “used” memory when there is a need to reuse it or when it is impacting other performance.**



Even if you skipped the previous bold section (which is perfectly acceptable), you might have noticed that when you clear out a variable, your memory usage for PowerShell might remain high for a while.&#160; My function speeds up the natural “garbage collection” process by calling <a href="http://msdn.microsoft.com/en-us/library/system.gc.collect.aspx" target="_blank">[System.GC]::Collect()</a>, forcing the CLR to reclaim that memory.



There are a couple of “helper” functions included, Get-SessionVariableList, which will list out the variable names saved, in case you wanted to see which variables were going to be left and Add-SessionVariable, which adds more values to the current list.&#160; 



&#160;



 <script type="text/javascript" src="http://PoshCode.org/embed/973"></script>  


Feedback and improvements are always welcome!



<a href="http://blogs.msdn.com/powershell/archive/2009/03/01/powershell-folksonomy.aspx" target="_blank">PSMDTAG</a>:FAQ Memory

