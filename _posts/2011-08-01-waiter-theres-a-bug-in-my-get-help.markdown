---
layout: post
title: "Waiter, There's a Bug in My Get-Help!"
date: 2011-08-15 07:00
author: steven.murawski@gmail.com
comments: true
categories: [CommandAssist, Modules, PowerShell Version 2]
tags: []
---


My friend <a href="http://robertrobelo.wordpress.com/about/" target="_blank">Robert Robelo</a> was kind enough to point out a problem with the proxies created by the CommandAssist module.



In trying to research the problem, I found that PowerShell crashed ( yes, crashed.. not throwing an error, but CRASHED) when Get-Help could not resolve the command it needed to provide help for.

### Setting the Stage 

#### Reproducing the Problem

Create a module that exports a function

```powershell
$Path = 'c:\users\smurawski\documents\windowspowershell\TestModule.psm1'$null = new-item $Path -ItemType File -Force -Value @'
function Test-One {
    [CmdletBinding()]
    param($x)
    $x
}

Export-ModuleMember -Function Test-One
'@
```

    Import the module
    
```powershell
Import-Module TestModule
```
    
    Create the proxy module
    
```powershell
Get-Module TestModule | 
    New-AssistedModule -ModulePath c:\users\smurawski\documents\windowspowershell\TestAssisted
```

    
    Import the proxy module
    
```powershell
Import-Module TestAssisted
```
    
    Try to Get-Help for Test-One (shell will crash here)
    
```powershell
Get-Help test-one
```

#### What's Happening?

In the proxy command that is generated, there is a help item (ForwardHelpTargetName) that is created to point the help file back to the original command.&#160; Since you could possibly have multiple commands, aliases, and functions with the same name, there is a second help entry (ForwardHelpCategory) that allows you to specify what type of command you are forwarding to.



**<u>NOTE:&#160; The following is my opinion of what is happening based on my testing.&#160; I haven't attached a debugger and watched to verify, but I have tested enough to confirm a workaround.&#160; This occurs in both the ISE and PowerShell.exe.&#160; I have not tested in other scripting environments.</u>**



In this case, both the original command and the proxy are functions, so Get-Help goes into a loop until it hits a recursion max and crashes PowerShell.&#160; 



### Get the Duct Tape 




#### Finding a Workaround




So, how can we get around this?&#160; CommandAssist relies on pointing back to the source command for the help files.&#160; 



Fully qualifying the command in the proxy will solve the problem.&#160; I've updated the proxy command generation to qualify the command in the ForwardHelpTargetName with the module prefixed name of the source command.



#### The Downside




If you've been playing with CommandAssist, you will want to regenerate your proxies.



### Let's Get This Fixed 




#### Vote it up on Connect!




I don't know about you, but I would like to see this type of error not crash PowerShell.&#160; <a href="https://connect.microsoft.com/PowerShell/feedback/details/684132/get-help-of-a-proxy-command-of-a-function-causes-crash" target="_blank">I've filed a Connect bug and would appreciate if you would head over and vote it up!</a>&#160; The team really does respond to community feedback and Connect is a great way to show that this is important to you!

