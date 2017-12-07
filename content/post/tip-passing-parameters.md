---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 1
- Tip
comments: true
date: 2009-03-01T00:00:00Z
tags: []
title: 'Tip: Passing Parameters'

---

Jeffery Hicks <a href="http://mcpmag.com/columns/article.asp?editorialsid=3028" target="_blank">recently talked about the Param statement in his Prof. Powershell column for MCP Magazine</a> (Keeping Your Options Open).  If you are new to PowerShell, I would definitely recommend reading the article, as provides a nice introduction to the **Param** statement. 
Jeffrey recommended to cast your parameter as the type of object that you are expecting, as way to catch errors.  I wholeheartedly agree.  That can cause trouble though, if you want to allow consumers of your script to pass in one or more values for that parameter.  In the spirit of the article’s title, I thought I would share a method I’ve come across to handle that situation.



Jeffrey gave is the below function.



Function Check-Service {
  Param([string]$computername=$env:computername,
    [string]$service="spooler" )
  $wmi=get-wmiobject win32_service -filter "name='$service'" -computername $computername
  if ($wmi.state -eq "running")  {
    write $True
  }
  else {
    write $False
  }
}



Now, you can easily see where you might want to check for a service on multiple computers, but as this function stands, we would have to call it one time for each computer we want to check.



We can modify this script to take one or more computer names simply by changing the cast for the $computername variable from **[string]** to **[string[]]**.  That changes makes our variable hold an array of strings, rather than just one. 



We don’t have to change the default value, as PowerShell handles the details of converting that value to an array. 



Since the –ComputerName parameter for Get-WMIObject can take an array of computer names, we don’t have to change much else in the function. 



I do add a foreach loop for the reporting end of the script, so we can see which machines have the service running.



And I end up with this (**my changes in bold**):



Function Check-Service



{



  Param( **[string[]]$computername**=$env:computername, [string]$service="spooler" )



  $wmi=get-wmiobject win32_service -filter "name='$service'" -computername $computername



  **foreach ($one in $wmi)**



**  {**



**    if ($one.state -eq "running") **



**    {**



**      write $one.SystemName,** $true



**    }**



**    else **



**    {**



**      write $one.SystemName,** $False



**    }**



**  }**



}



 



<a href="http://blogs.msdn.com/powershell/archive/2009/03/01/powershell-folksonomy.aspx" target="_blank">I almost forgot </a>- 



PSMDTAG:FAQ param

