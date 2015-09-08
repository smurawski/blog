---
layout: post
title: "Working the Pipeline"
date: 2011-05-16 08:15
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames, parameters]
---


The pipeline is a central concept in PowerShell.&#160; It enables you to chain the output from one command to the input of the next, making automation much smoother.&#160; Making your scripts and functions play nicely with the pipeline can mean really impact the ease of use and adaptability of your functions. 



## Finding the Value




In scripts and advanced functions, declaring a parameter whose value can come from the pipeline is pretty straightforward.&#160; You can specify whether you are looking for the full object



[parameter(ValueFromPipeline=$true)]</pre>

    
    or if you would like the value to be filled in by a property on an incoming object
    
<pre language="powershell">[parameter(ValueFromPipelineByPropertyName=$true)]</pre>

    
    # 
    
    

    
    ## The Hidden Value
    
    

    
    When you enable a parameter to take a value from the pipeline (or a property on the pipeline), you gain a hidden benefit - Scriptblock Parameters.
    

    
    ### Transforming Input
    
    

    
    Scriptblock parameters allow you to modify the input to a parameter based on the incoming object by specifying a scriptblock instead of a value to a parameter.
    
<pre language="powershell">Get-ChildItem C:\TestFiles\*.doc | Rename-Item -NewName {&quot;New-$($_.name)&quot;}



That's a pretty trivial example, but since we are providing a scriptblock, we can actually do anything that PowerShell allows in defining a new input for that parameter.

