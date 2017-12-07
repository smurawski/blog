---
author: steven.murawski@gmail.com
categories:
- PowerShell Version 2
- PowerShell Version 3
- PowerShell Version 3 CTP 1
- PowerShell Version 3 CTP 2
comments: true
date: 2012-01-01T00:00:00Z
tags: []
title: When Import-Module Does Not

---

A question came up about the behavior of [what’s that](/blog/2012/01/powershell-v3-auto-loading-of-modules)?).



### So What’s the Question?




Does the [auto-loading of modules](/blog/2012/01/powershell-v3-auto-loading-of-modules) in PowerShell Version 3 use Import-Module behind the scenes?&#160; If it does, can we use [default parameter sets](/blog/2012/01/powershell-v3-default-parameter-values) to control what types of things are imported?



### What Actually Happens?




So, it turns out that [default parameter set (new feature)](/blog/2012/01/powershell-v3-default-parameter-values) can be used to customize the import of new modules.



$PSDefaultParameterValues = @{    &quot;Import-Module:Alias&quot;=@()
    &quot;Import-Module:Function&quot;='*'
    &quot;Import-Module:Cmdlet&quot;='*'
    &quot;Import-Module:Variable&quot;='*'
}
Get-Module
Get-BitsTransfer
Get-Module</pre>

    
    This example (when used in PowerShell V3) will (from a base PowerShell session) show you what modules are loaded, attempt to run the Get-BitsTransfer command, and then show you what modules are now loaded (should include the BitsTransfer module now).&#160; Since we’ve provided an empty array to the Alias parameter for <a href="http://go.microsoft.com/fwlink/?LinkID=141553" target="_blank">Import-Module</a>, any module that is imported will import any aliases.
    

    
    ### Seems Like It Works… Where’s the Problem?
    
    

    
    The problem comes in when you specify that only one of the four parameters (Alias, Function, Cmdlet, and Variable).
    

    
    You might expect that when you specify one parameter (example Alias), that the other portions of the import (Function, Cmdlet, and Variable) would behave just like if you did not specify any of them. 
    

    
    Example -
    

    
    Open a new PowerShell session (V2 or V3) -
    
<pre language="powershell">

Import-Module BitsTransfer
Get-Command -Module BitsTransfer

Get-Module
</pre>

    
    This will import BitsTransfer module and all attendant commands and functions.&#160; Note the ExportedCommands in the Get-Module output.
    

    
    Open a new PowerShell session and try -
    
<pre language="powershell">

Import-Module BitsTransfer -Alias @()
Get-Command -Module BitsTransfer

Get-Module




This should import the BitsTransfer without any aliases (yes, I know BitsTransfer doesn’t have any aliases, but I wanted a module that most any Windows 7 or Server 2008 R2 machine would have – feel free to test with whatever module you desire).&#160; 



This did import the BitsTransfer, but no commands or variables were imported.&#160; It only attempted to import aliases, and since we specified that aliases were to be an empty array, nothing was imported.



**This is counter-intuitive and is not the experience we should have.&#160; Specifying one parameter should not change the default value of other parameters (from all to none in this case).**



### What Can I Do About It?




First of all, open PowerShell and try it!&#160; If you have one of the CTPs of Version 3, try it there.&#160; Also try it in V2 using <a href="http://go.microsoft.com/fwlink/?LinkID=141553" target="_blank">Import-Module</a> directly. 



Then go to Microsoft Connect and vote up [https://connect.microsoft.com/PowerShell/feedback/details/716857/module-partially-loads-with-import-module](https://connect.microsoft.com/PowerShell/feedback/details/716857/module-partially-loads-with-import-module). 

