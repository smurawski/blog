---
layout: post
title: "Using Pipeline Input"
date: 2011-04-10 15:56
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames, parameters]
---


In judging the <a href="http://bit.ly/2011sgall" target="_blank">Scripting Games</a>, I've found a good number of scripts that are trying to use the Parameter attribute to take pipeline input, but there are several key mistakes.



## Why Doesn't This Work?




### The Problem




When you specify a parameter as taking a value from the pipeline, that parameter name is the variable name that you refer to below.&#160; 



function Write-SampleData (){
    param(
        [Parameter(ValueFromPipeline=$true,Position=0)] [string] $SampleData
    )

    Write-Host $SampleData
}
'TestData', 'TestData2', 'TestData3' | Write-SampleData</pre>

    
    When you run the above, you only see the output from the last item in.
    

    
    ### Compensating
    
    

    
    So, to compensate for that, I've seen a few approaches, but the most common correction I've seen is to use the $input variable. 
    
<pre lang="powershell">function Write-SampleData ()
{
    param(
        [Parameter(ValueFromPipeline=$true,Position=0)] [string] $SampleData
    )

    $SampleData = $input

    Write-Host $SampleData
}
'TestData', 'TestData2', 'TestData3' | Write-SampleData</pre>

    
    In doing so, you defeat the purpose of the parameter attribution.&#160; You can also overwrite any values that would have been assigned to $SampleData.
    

    
    ## How Can I Fix This And Still Get Pipeline Input?
    
    

    
    The fix comes down to understanding what is happening when scripts and functions get pipeline input.
    

    
    When you specify that a parameter takes pipeline input, that input is evaluated for every run of the &quot;Process&quot; block (see the <a href="http://technet.microsoft.com/en-us/library/dd347712.aspx" target="_blank">about_Functions help</a> for further details on the Begin, Process, and End blocks).
    

    
    Changing the script to put the action in the Process block will give us the desired effect.
    
<pre lang="powershell">function Write-SampleData ()
{
    param(
        [Parameter(ValueFromPipeline=$true,Position=0)] [string] $SampleData
    )

    process
    {
        Write-Host $SampleData
    }
}
'TestData', 'TestData2', 'TestData3' | Write-SampleData



Leave the $input for the older, PowerShell V1 style scripts and use the Process block and the ValueFromPipeline or ValueFromPipelineByPropertyName.

