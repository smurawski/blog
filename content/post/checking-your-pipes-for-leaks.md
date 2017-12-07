---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 2
comments: true
date: 2011-09-01T00:00:00Z
tags: []
title: Checking Your Pipes For Leaks

---

(NOTE: I started this post a few weeks ago)



This morning I woke up with my kitchen floor covered in water.&#160; As I was cleaning it up, I couldn't help drawing some parallels between fixing a leaky pipe and a &quot;leaky&quot; (non-performing) script.



My repair process this morning took the following steps:



1.  Contain the leak 
2.  Locate the source of the leak 
3.  Stop the leak 
4.  Check for related damage 
5.  Mitigate the related damage 
6.  Patch the leak&#160; 
7.  Confirm the leak is fixed 
8.  Monitor the leak site 


Since most of you won't really care about the plumbing in my kitchen, let's run through how dealing with this problem applies to troubleshooting a misbehaving script (or function).



### Containing the Leak




The first step I needed to take was to contain the leak.. in PowerShell, that means I need to pull that script or function from general use, as well as terminating any running instances.



This can be tougher than you think, especially if you are using remote jobs or are performing a sequential series of actions that cannot be rolled back (especially if you don't have good logging in place!).



### Locating the Source




After I’ve got the leak contained, I had to find where the leak was coming from.&#160; For my misbehaving script, I either need to find what has failed.&#160; Normally, the errors that I'm getting will help point me towards the source of the problem.&#160; 



However, if I'm not getting errors, I need to step through the script (or function) and can use Set-PSBreakpoint to create an entry point in my script where I want to start walking through.&#160; 



If my script (or function) is an advanced function, I could use -Debug and have it break on any Write-Debug statements that I've used throughout my script.&#160; This is a GREAT reason to have a periodic Write-Debug statement before any significant blocks of logic in your script.



### Stopping the Leak




Once I've found the source of the leak, I can begin to stop the leak.&#160; In my kitchen leak, I had to turn off the water. 



In the case of my script, I have to find any scheduled tasks using it and stop them or advise other people who might be using the script to stop using it until I can publish a new script.



### Checking for Damage




Once I had my leak stopped, I started to check for residual damage that I might have to control.&#160; Water had leaked onto where the power cord for my water heater plugged into an extension cord,&#160; This caused the <a href="http://en.wikipedia.org/wiki/Ground_fault_circuit_interrupter" target="_blank">GFI outlet</a> to trip. 



Collateral damage from a misbehaving script or function can be disastrous.&#160; Very often, scripts are run under administrative credentials and can have unexpected side effects.&#160; When locating the source of the leak, I’ll check to see what other systems or areas that the script is touching and examine them for potentially improper changes or updates.&#160; Finally, I need to document any “damage” or errors that the script created.



### Mitigating the Damage




Once I found that the GFI was tripped, I unplugged everything on that chain.&#160; I let everything dry out and replaced the extension cord.



For a misbehaving script, this is going to vary based on what the script was actually doing, but this can be one of the most important steps.&#160; I have to record all the actions I take to mitigate the “damage” found in the previous step.&#160; Even if I decide not to do anything about the damage I found in the previous step, that is still an action and should be documented.



### Patching the Leak




Patching the leak is pretty straightforward.&#160; Going to the source of the leak, I removed the faulty tap and patched the pipe.



In my script, that means going to the source of the faulty logic or source of the errors and either fixing the logic and/or adding error handling to deal with the source of the errors.&#160; I’ll also add additional Write-Verbose or Write-Debug statements to make tracing problems and verifying functionality easier.&#160; For scripts that will 



### Confirming the Leak is Fixed




After I patched the leak, I turned the water back on and watched it for a short while to make sure that there was no seepage from the patch.&#160; I tested down-level plumbing to make sure that the pressure was appropriate as well.



After I’ve added my error handling and/or fixed the logic, I’ll run the script again, perhaps with –Verbose or –Debug.&#160; If I’m really motivated, I’ll make sure there is the CmdletBinding attribute and add –WhatIf support.



### Monitoring the Leak




After I was able to confirm that the leak was fixed, I stayed home and periodically checked the line to watch for any seepage or residual problems.&#160; Over the next few days, I continued to monitor the site of the patch for any issues, though not as frequently.



After I’ve fixed my script, I’ll continue to watch was it gets used, perhaps logging all the output, until I’ve regained confidence in the project.

