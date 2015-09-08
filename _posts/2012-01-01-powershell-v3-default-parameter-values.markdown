---
layout: post
title: "PowerShell V3 – Default Parameter Values"
date: 2012-01-06 10:45
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 3, PowerShell Version 3 CTP 1, PowerShell Version 3 CTP 2]
tags: []
---


PowerShell Version 3 introduces the concept of Default Parameter Values.



This allows you to specify a value for one or more parameters for one or more commands.



To implement this, you need to populate a new hashtable – $PSDefaultParameterValues



### That Description is Not Helpful




Let’s look at some examples - 



First off, we can specify the value for a parameter for a specific command.&#160; The pattern used for the hashtable entries is *“NameOfCommand:NameOfParameter” = “ValueOfParameter”*.



(I’m stealing some of these examples from the samples provided in CTP1.)





$PSDefaultParameterValues = @{&quot;Get-Process:Name&quot;=&quot;powershell&quot;}

Get-Process

Get-Process e*
</pre>

    
    With that set, every time we run Get-Process and do not specify the Name parameter, the PowerShell runtime will stick “powershell” in for that value.&#160; A specified parameter will always override a default value.
    

    
    Let’s go the other way and, instead of specifying one command, set a parameter value on all commands (that have that parameter.
    
<pre language="powershell">

$cred = Get-Credential mydomain\mydomainadminaccount

$PSDefaultParameterValues = @{&quot;*:Credential&quot; = $cred}
</pre>

    
    This would provide your $cred to any Credential parameter.
    

    
    We can also use wildcards to match multiple commands.
    
<pre language="powershell">$PSDefaultParameterValues = @{&quot;*-AD*:Server&quot; = 'MyDomainController'}</pre>

    
    This would provide a default value to the Server parameter for any command whose noun started with AD (like everything in the ActiveDirectory module.
    

    
    Since this is a hashtable, it can hold many different mappings - 
    
<pre language="powershell">

$PSDefaultParameterValues = @{

  &quot;*:Credential&quot; = $cred

  &quot;Get-Process:Name&quot;=&quot;powershell&quot;

  &quot;*-AD*:Server&quot; = 'MyDomainController'

}
</pre>

    
    To turn off DefaultParameterValues (without removing everything you have set up), you can set a key “Disabled” equal to $true.
    
<pre language="powershell">$PSDefaultParameterValues += @{&quot;Disabled&quot; = $true}



This concludes our brief look at DefaultParameterValues for today.



Go grab <a href="http://www.microsoft.com/download/en/details.aspx?id=27548" target="_blank">Version 3 CTP2</a> and give it a whirl…

