---
layout: post
title: "Exploring the .NET Framework with PowerShell – Garbage Collection (Part 6)"
date: 2009-07-07 06:00
author: steven.murawski@gmail.com
comments: true
categories: [-NET Framework, PowerShell]
tags: []
---


Garbage collection is a process that the .NET Framework (upon which the PowerShell runtime works) uses to manage memory.&#160; The garbage collection (for applications – services are handled a bit differently) process basically covers X steps:



1.  Identify objects that won’t be used 
2.  Delete them from memory 
3.  Compact the space to make room for new objects 


The first step is the critical point for PowerShell users.&#160; You may have noticed how the memory used in a PowerShell session can steadily climb (or suddenly climb if you’re dealing with a large number of objects).&#160; I talked about that in [this tip](/blog/2009/03/tip-free-up-some-memory).&#160; The .NET garbage collector looks for objects that are still in use or could possibly be in use in the near future (this is a gross oversimplification - if you are really interested in a detailed explanation of how garbage collection works in the .NET Framework, <a href="http://www.simple-talk.com/content/article.aspx?article=737" target="_blank">check out this article</a>.) 



What this means to scripters who are running into memory pressure issues is that the garbage collector will not reclaim any objects you have actively referenced (meaning that you’ve assigned them to a variable) in your current or higher scope.&#160; 



Since this can sound a bit convoluted let’s look at an example.&#160; 



I read a large file to parse it and leave the contents of the original stored in a variable



PS&gt; $MyLargeFile =&#160; [Get-Content](http://go.microsoft.com/fwlink/?LinkID=113310) MyLargeFile.txt    <br>PS&gt; Foreach ($line in $MyLargeFile) {    <br>DoSomething $line | [Out-File](http://go.microsoft.com/fwlink/?LinkID=113363) NewUpdatedLargeFile.txt    <br>}



One of the problems with the above pattern is that the original file remains saved in memory and doesn’t get released unless you explicitly remove the reference, either by using [Remove-Variable](http://go.microsoft.com/fwlink/?LinkID=113380) or [Remove-Item](http://go.microsoft.com/fwlink/?LinkID=113373) or by changing what $MyLargeFile points to by assigning another value to it.&#160; 



This isn’t VBScript where you have to set values to “Nothing”, but you should be aware of what larger object sets you are retaining in memory.&#160; 

