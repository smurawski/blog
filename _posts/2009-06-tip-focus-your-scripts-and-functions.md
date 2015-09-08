---
layout: post
title: "Tip: Focus Your Scripts and Functions"
date: 2009-06-02 06:30
author: steven.murawski@gmail.com
comments: true
categories: [General, PowerShell, Tip]
tags: []
---


The PowerShell paradigm is the task based cmdlet.&#160; With cmdlets that surface a single function that handle a multitude of inputs, a PowerShell session or script can read back like a sentence.&#160; 



Get-Process –Name iexplore | Where-Object {$_.WorkingSet –gt 50000000 } | Stop-Process



In the above (often overused, but illustrates the point well) example, Get-Process, Where-Object, and Stop-Process all accurately describe the task that they perform.



We can take this same approach with our scripts and functions.&#160; By keeping your scripts and functions focused on performing discreet tasks, you keep with the composable nature of PowerShell and provide yourself the most flexibility in reusing the functions you create.



One of the most common “errors” I see in PowerShell scripts that are shared is the embedding of formatting in the output of the script.&#160; If your script generates output, it should be in the form of objects (custom or otherwise).&#160; (The only caveat here is if your script’s functionality is to handle the output of data, in which case it’s sole focus should be the handling that output.)



The PowerShell ecosystem provides a number of tools for formatting the output of your scripts, whether you want to display your scripts in the console window (<a href="http://technet.microsoft.com/en-us/library/dd347700.aspx" target="_blank">Format-List</a>, <a href="http://technet.microsoft.com/en-us/library/dd315387.aspx" target="_blank">Format-Wide</a>, <a href="http://technet.microsoft.com/en-us/library/dd315392.aspx" target="_blank">Format-Custom</a>, <a href="http://technet.microsoft.com/en-us/library/dd315255.aspx" target="_blank">Format-Table</a>), as a web page (<a href="http://technet.microsoft.com/en-us/library/dd347572.aspx" target="_blank">ConvertTo-HTML</a>), sent to a CSV file (<a href="http://technet.microsoft.com/en-us/library/dd347724.aspx" target="_blank">Export-CSV</a>) or XML (<a href="http://technet.microsoft.com/en-us/library/dd347657.aspx" target="_blank">Export-CLIXML</a>), in Version 2 to a sortable, groupable grid (<a href="http://technet.microsoft.com/en-us/library/dd315268.aspx" target="_blank">Out-GridView</a>), as well as a number of other options (<a href="http://huddledmasses.org/powerboots-output-graphs-to-images-from-powershell/" target="_blank">Visifire graphs via PowerBoots</a>, <a href="http://chadwickmiller.spaces.live.com/blog/cns!EA42395138308430!340.entry" target="_blank">Excel, PDF, or Image via the Reporting Services Redistributable</a>, or <a href="http://dougfinke.com/blog/index.php/2008/08/31/microsoft-research-netmap-and-powershell/" target="_blank">as a netmap using NodeXL</a>).



Take advantage of these, or write your own.&#160; If you write your own, it should work with custom objects, to keep with the composable nature of PowerShell.

