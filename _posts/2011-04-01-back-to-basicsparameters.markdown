---
layout: post
title: "Back to Basics–Parameters"
date: 2011-04-25 14:31
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 1, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames, parameters]
---


My previous posts about the <a href="http://bit.ly/2011sgall" target="_blank">Scripting Games</a> had many references to how parameters were used in scripts and functions.&#160; Since those posts, I’ve had several requests to expand on that, so here we go…



## In the Beginning…




In PowerShell V1, when we wrote scripts or functions, we had two main ways to retrieve parameters.



### The Dark Ages




First off, we could parse $Args.



function Do-Something ()
{
    $FirstArgument = $args[0]
    $SecondArgument = $args[1]

    ...</pre>

    
    This was a particularly bad approach (in most cases), since users did not get any help from tab completion on parameter names and they had to get the right order to make things work (meaning they had to be very familiar with the order of things required.
    

    
    ### The Renaissance 
    
    

    
    PowerShell promised us a better, more enlightened manner of getting input into our functions and scripts, declaring parameters.
    
<pre language="powershell"> 
function Do-Something ()
{
    param ($FirstArgument, $SecondArgument)
    ...</pre>

    
    Declaring our parameters like this allows a user to specify the parameter name and let a user take advantage of tab completion to find the parameters allowed.&#160; Parameters could be specified in any order, so long as there was enough information to match them up.
    

    
    Since PowerShell is an object-based environment and can use .NET, we also gained the ability to specify the type of input we were expecting.&#160; This lets us catch errors much sooner (for example if we were expecting a number and someone tried to pass in “hello”) and gives the user immediate feedback that something is not right.
    
<pre language="powershell"> 
function Do-Something ()
{
    param ([string]$FirstArgument, [int]$SecondArgument)
    ...</pre>

    
    One of the final benefits that this syntax allows for is that we can specify a default value (or through some trickery create a mandatory parameter).
    
<pre language="powershell"> 
function Do-Something ()
{
    param ([string]$FirstArgument=&quot;PowerShell Rocks&quot;,
     [int]$SecondArgument = $(throw &quot;I'm Required!!&quot;)
    ...</pre>

    
    ## Entering the Modern Day
    
    

    
    ### The Industrial Revolution
    
    

    
    Version 2 of PowerShell kicks parameters into high gear.&#160; All the cool things that developers could do in managed code writing a PowerShell cmdlet were now in the hands of scripters.
    

    
    By building on to the parameter declarations and adding one or more “<a href="http://technet.microsoft.com/en-us/library/dd347600.aspx" target="_blank">attributes</a>”, we can do all sorts of wonderful things.&#160; (This is by no means an exhaustive list.&#160; Check the <a href="http://technet.microsoft.com/en-us/library/dd347600.aspx" target="_blank">about_Advanced_Parameters</a> help topic for further information.)
    

    
    We can 
    


*   specify that a parameter is mandatory (without the trickery demonstrated earlier).&#160; 
<pre language="powershell"> 
[parameter(mandatory=$true)][string]$FirstParameter</pre>

*   assign a position so that arguments can be passed without a parameter name (for brevity on the command line). 
<pre language="powershell"> 
[parameter(mandatory=$true, position=0)][string]$FirstParameter</pre>

*   take input from the pipeline and assign it to a parameter. 
<pre language="powershell">[parameter(mandatory=$true, ValueFromPipeline=$true)][string]$FirstParameter</pre>

*   take a value from a property in pipeline input and assign it to a parameter. 
<pre language="powershell">[parameter(mandatory=$true, ValueFromPipelineByPropertyName=$true)][string]$Name</pre>

*   add aliases (in case those property names don’t line up exactly). 
<pre language="powershell">[parameter(Mandatory=$true, ValueFromPipelineByPropertyName=$true)]
[alias('Server','__Server', 'Name')]
[string]
$ComputerName</pre>

*   assign any values that are not matched up to a parameter. 
<pre language="powershell">[parameter(ValueFromRemainingArguments=$true)][string]$Name</pre>

*   add a help message. 
<pre language="powershell">[parameter(HelpMessage=&quot;I want a name here&quot;)][string]$Name</pre>

*   add some validation (many options here.. see the help for them all).&#160; 
<pre language="powershell">[parameter(mandatory=$true, ValueFromPipelineByPropertyName=$true)]
[ValidateNotNullOrEmpty()]
[string]
$Name</pre>


    
    These different options throw open the doors for all sorts of cool scenarios and greatly reduce the amount of work we as scripters need to do to take input into our scripts and functions.
    

    
    ### The Space Age
    
    

    
    Once we’ve figured out how to make our parameters act like those of cmdlets, we can begin to leverage the same techniques we use with cmdlets (like scriptblock parameters).&#160; 
    

    
    Scriptblock parameters allow you to transform your pipeline input to any parameter that takes a value from the pipeline by property name.&#160; 
    

    
    For example, if I wanted to copy and rename a file based on the current file name, I could do something like 
    
<pre language="powershell"> 
Get-childitem ~\documents\windowspowershell\*.ps1 | copy-item -destination {$_.fullname -replace 'windowspowershell','backup'}



Instead of having to transform the location inside of a foreach loop or create a mapping of the old location and new location, I can pass that transformation as an argument to the command and it will evaluate that for each item piped in. 



My example uses cmdlets, but when I add the pipeline related parameter attributes to my parameters in my script or function, I gain that capability as well.

