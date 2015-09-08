---
layout: post
title: "Script Injection with Set-PSBreakpoint"
date: 2010-01-12 14:15
author: steven.murawski@gmail.com
comments: true
categories: [General, PowerShell, PowerShell Version 2]
tags: []
---


I’ve used the integrated debugging features with <a href="http://support.microsoft.com/kb/968929" target="_blank">PowerShell V2</a> since I’ve had it available, but I never really dug below the surface of setting breakpoints at certain lines.



<a href="http://technet.microsoft.com/en-us/library/dd315264.aspx" target="_blank">Set-PSBreakpoint</a> offers some additional options of which I was not aware.



1.  **Setting a breakpoint in relation to a variable (read, assigned, or both)** 
2.  **Setting a breakpoint when a function or command is called** 
3.  **Setting a breakpoint based on the column number for the referenced line** 
4.  **Run a scriptblock when a breakpoint is hit** 
5.  **Breakpoints do not need to be set on a script** 


Let’s dig into these in a bit more detail:



### 1.  Setting a breakpoint in relation to a variable (read, assigned, or both)




Set-PSBreakpoint can be used to find all the occurrences of access to a variable (in the current scope).  This can be very useful when attempting to find out where things might be taking an unexpected turn with your variable’s contents.



### 2. Setting a breakpoint when a function or command is called




This is cool.  You can configure breakpoints based on when cmdlets or functions are called.  Great stopping at the entry point to a particularly troublesome function, so you can drop into the debugger and check the state of parameters about to go in, as well as other state related issues.



### 3. Setting a breakpoint based on the column number for the referenced line




I’m not so stoked about this feature.  This merely allows you to specify which column to stop execution on in a particular line of code…  Moderately useful, but not really exciting.



### 4. Run a scriptblock when a breakpoint is hit




This is where things get interesting.  You can assign an action to occur when a breakpoint is hit.  This action is a scriptblock that is run in the scope where it is set.  Since breakpoints can be variable assignments or calls to commands, this opens up some interesting possibilities.  First off, it allows for conditional debugging.  If you only want to drop into a breakpoint if a particular value is less than zero before going into a function, you could do something like



<span style="color: #ff4500">$BreakpointAction</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">{</span>    <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$MyNumber</span> <span style="color: #a9a9a9">-lt</span> <span style="color: #800080">0</span><span style="color: #000000">)</span>
    <span style="color: #000000">{</span>
        <span style="color: #00008b">break</span>
    <span style="color: #000000">}</span>
    <span style="color: #00008b">else</span>
    <span style="color: #000000">{</span>
        <span style="color: #00008b">continue</span>
    <span style="color: #000000">}</span>
<span style="color: #000000">}</span>



This also has applications outside of debugging.  Using the –Action parameter of Set-PSBreakpoint, you have the ability to run a scripblock of your choosing at any of the condition types described above – when variables are accessed, when commands are called, and at certain specific positions in the script.



 



### 5. Breakpoints do not need to be set on a script




Finally, breakpoints do not need to be set in a script, they can just be set to respond to variable access or command use.  This means that you could use Set-PSBreakpoint in a profile script to configure a particular environment to respond in a certain way, perhaps prompting you before changing a critical environmental variable.



 



I’m definitely going to be exploring these additional features and applications of Set-PSBreakpoint as I go forward. 



<a href="http://blogs.msdn.com/powershell/archive/2009/07/13/advanced-debugging-in-powershell.aspx" target="_blank">Additional debugging tips/info from the PowerShell Team Blog.</a>



Please leave a comment as to how you think this functionality could be used.

