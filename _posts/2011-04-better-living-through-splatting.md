---
layout: post
title: "Better Living Through Splatting"
date: 2011-04-14 05:00
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames]
---


One pattern I've noticed, especially when dealing with commands that may or may not be executed locally is where a script offers a parameter for the computer name (or a list of computer names). 



## What Are People Doing?




Sometimes, that parameter will be defaulted to $env:ComputerName



[Parameter(Mandatory=$false, Position=1, ValueFromPipeline=$true)][string]
$ComputerName = $env:ComputerName</pre>

    
    Other times, there will be an if statement checking to see if the parameter is null and assigning $env:ComputerName if it has no other value.
    
<pre language="powershell">if ($ComputerName -eq $null)
{
  $ComputerName = $env:ComputerName
}</pre>

    
    From there, the script will go on to do something like 
    
<pre language="powershell">$Result = Get-AllWonderfulAndGreatThings -ComputerName $ComputerName -Filter * -Mood Happy -ErrorAction SilentlyContinue</pre>

    
    ## How Can I Be Cooler?
    
    

    
    [Splatting can offer another option.](/blog/2011/04/clean-up-your-parameters)&#160; We still need to do a null check, but we can build up a hash table with the known parameters we would like to pass, and if $ComputerName exists, we can tack that on later.&#160; We can then pass the whole thing to the command in one fell swoop.
    
<pre language="powershell">$ParametersToPass = @{
  Filter = '*'
  Mood = 'Happy'
  ErrorAction = 'SilentlyContinue'
}
if ($ComputerName -ne $null)
{
  $ParametersToPass.Add('ComputerName', $ComputerName)
}
$Result = Get-AllWonderfulAndGreatThings @ParametersToPass



## But, What if I Have Other Conditional Parameters?




Go for it.. this technique can be used to programmatically build function, cmdlet, and script calls for any sort of parameter.&#160;&#160; 

