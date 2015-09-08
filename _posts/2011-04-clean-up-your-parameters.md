---
layout: post
title: "Clean Up Your Parameters"
date: 2011-04-11 21:28
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames, parameters]
---


Another tidbit to come from the scripts I've been reviewing is how messy commands with tons of parameters can be.&#160; You end up with really long lines, uneven lines, and/or lots of backticks.



## But What Can We Do?




But there are commands that require many parameters or long parameters.&#160; We can only stuff so much in variables, and if we name them clearly, they tend to be longer too.&#160; So how can we deal with this?



## Splat 'Em!




Version 2 of PowerShell introduces a concept called &quot;splatting&quot;.&#160; What that means is that I can take a hashtable with keys that are the names of parameters and use that to pass input to my function, script, or cmdlet. In order to make this magic happen, I have to change th &quot;$&quot; in the variable indicator to &quot;@&quot; when I supply that hashtable to the command.



This…



Invoke-Command -ComputerName $MyDomainController -Credential $DomainAdminCreds -ConfigurationName ActiveDirectoryAdmin -JobName GettingWorkDone -AsJob -ScriptBlock {get-aduser -filter *}</pre>

    
    Becomes this..
    
<pre lang="powershell">$MyCommandParameters = @{
    ComputerName = $MyDomainController
    Credential = $DomainAdminCreds
    ConfigurationName = 'ActiveDirectoryAdmin'
    JobName = 'GettingWorkDone'
    AsJob = $true
    ScriptBlock = {Get-ADUser -filter *}
}
Invoke-Command @MyCommandParameters



Much more readable (to me at least)!

