---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 2
- Scripting Games
- Tip
comments: true
date: 2011-05-01T00:00:00Z
tags:
- 2011ScriptingGames
- naming
- parameters
- variables
title: Back to Basics - Variable Names

---

$x and $z and $foo just aren't going to cut it in your production scripts.&#160; 



<a href="http://get-powershell.com/" target="_blank">Andy Schneider</a> <a href="http://twitter.com/#!/andys146" target="_blank">tweeted</a> during the <a href="http://bit.ly/2011sgall" target="_blank">Scripting Games</a> that there was no penalty for providing a descriptive variable name.&#160; He was right and not quite right enough.&#160; Twitter does not provide you the opportunity to share more than a sound bite in one tweet, so he really could not have commented much further there, <a href="http://get-powershell.com/post/2011/04/13/Extra-Points-for-Style-when-writing-PowerShell-Code.aspx" target="_blank">but he did in a post that expanded on what judges were looking for in style for the Scripting Games</a>.&#160; 



The content of that post is not limited to the Scripting Games, but should be applied to our scripting experience.



## Down to Business




We're going to focus on one particular section of that blog post.



>

Use variables that make it easy to understand what you are doing in your script. Comments in code are great, but your code should be readable and understood without them. In general, I would choose good variable names and clear processes over heavily commented code.






### Looking for Trouble




If you find yourself working on a script or function and begin to think, &quot;I need a comment here to make sure it is clear what I'm doing,&quot; then you are already headed for trouble.&#160; (Function, cmdlet, and script names are another portion for another day.)



Descriptive variable names can serve as inline documentation on what that variable represents.&#160; A descriptive variable makes it much easier to follow that variable through the script.&#160; 



Remember that you might not be (and will likely not be) the last person to look at that script and need to figure out what is going on internally.&#160; 



If by chance you are the last person to look at that script, do you think you are going to be able to have the same contextual understanding one month after you wrote the script?&#160; Two months?&#160; Six months or more?&#160; I know that when I review scripts I wrote years ago, before I followed my own advice better, it can take me a while to figure out what is really going on, even in some medium complexity scripts (where I should really just be able to scan and understand).



### Why Aren't Comments Good Enough




Comments are not the workaround for poor variable naming.&#160; Scripts are rarely static entities.&#160; As requirements change, scripts are updated, sections of functionality are changed and the objects that are operated on can change as well.&#160; Comments are often the last thing to be reviewed (if at all), so the likelihood of ending up with stale and misleading comments goes up with each revision.



Another reason that comments aren't good enough is that they clutter up the script file.&#160; Comment-based help is one thing (and that is localized to one place in the file, not spread across the script.&#160; When I'm reading a script and there are lines upon lines of comments, it really gets in the way of understanding what is happening in the script.&#160; PowerShell is rather unique in the case, since the syntax actually lends itself well to self-documenting code (with how the command naming works).&#160; 



### Naming Guidelines




These are some of the guidelines I try to follow when naming variables. 



#### Do Not:




*   Pluralize variable names that are not arrays.
*   Pluralize variable names that are used as parameters.
*   Reuse counter variable names.&#160; If you have a variable acting as a counter for a loop, so not reuse that variable name (you may have unexpected results if you don't clean up after the previous loop).
*   Use generic names like $Results or $Output.
*   Use custom abbreviations.&#160; Example: $CN should not be used for $ComputerName.


#### Do:




*   Describe the instance (or instances) of object(s) contained in the variable
*   Reference similar values consistently.&#160; If you have a variable $ComputerName which has an array of computer names, don't also have a variable $server or $servers that refer to the same data.
*   Use singular nouns.&#160; This maintains consistency with parameter names which by best practice are singular.&#160; The PowerShell convention is that variables with a singular noun can contain one or more objects.
*   Consider that you might be reading this script at 3 in the morning during an active outage.&#160; Pick names that will help you know what is going on and get your work done, so you can go home.
*   (optional) Use company and industry accepted abbreviations or acronyms.&#160; (For example $VM conveys the same information $VirtualMachine to systems administrators.


I'd like to hear what guidelines you've followed or what you thing of mine, so post a comment!

