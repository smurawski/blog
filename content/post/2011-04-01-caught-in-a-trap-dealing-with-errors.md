---
author: steven.murawski@gmail.com
categories:
- Error Handling
- PowerShell Version 2
- Scripting Games
- Tip
comments: true
date: 2011-04-01T00:00:00Z
tags:
- 2011ScriptingGames
title: Caught in a Trap - Dealing with Errors

---

Errors occur.&#160; Things fail.&#160; As an author of a script, you need to be able to deal with these hiccups.



## What Do I Need To Watch For?




PowerShell has different types of errors ([I explain some of that here.](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf51ae4b0c945709cff50/1353512218638/?format=original))&#160; Terminating errors can ruin a script's day.&#160; Say you are trying to gather information or update settings to a large number of servers.&#160; What happens when you hit an error on server number three, or number 33, or number 333?&#160; You'll know you hit an error on that item, 



*   What about the remaining servers?&#160; 
*   Do you need to re-run the script?&#160; 
*   What about the servers it completed on?&#160; Can you run it there again?


## It Depends




How much any of this matters really depends on your script and what you are trying to accomplish, but you really do need to take errors into account, especially if you are handing your script off to someone else.



## What can I do?




Take advantage of the error handling syntax that PowerShell provides.&#160; Wrap potentially troublesome code in a try/catch or try/catch/finally block (check out the help for about_Try_Catch_Finally).&#160; It can be as simple as 



param (
    [parameter()]
    [string[]]
    $ComputerName
    )

process 
{
    if ($ComputerName.count -gt 0) 
    {
        try 
        {
            Get-Process -ComputerName $ComputerName -Name powershell
        }
        catch 
        {
            Write-Error "Error trying to connect and retrieve process from $ComputerName"
        }
    }
}


