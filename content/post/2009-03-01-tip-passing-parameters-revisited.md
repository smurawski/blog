---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 1
- Tip
comments: true
date: 2009-03-01T00:00:00Z
tags: []
title: 'Tip: Passing Parameters Revisited'

---

In [this post](/blog/2009/03/tip-passing-parameters).  Jeff describes how you can make a function act more like a cmdlet.
The problem that you run into in V1 of PoweShell is that it is easy to make your functions work with the pipeline



**Function Check-Service {
  Param([string]$service=”spooler” )
  PROCESS
  {
    $wmi=get-wmiobject win32_service -filter “name=’$service’” -computername $_ 
    if ($wmi.state -eq “running”)  {
      write $True
    }
    else {
      write $False
    }
  }
}
 
Get-Content servers.txt | Check-Service**



*OR* work well with parameters being passed in.



**Function Check-Service {
  Param([string[]]$server=$env:computername, [string]$service=”spooler” ) 
    $wmi=get-wmiobject win32_service -filter “name=’$service’” -computername $server 
    if ($wmi.state -eq “running”)  {
      write $True
    }
    else {
      write $False
    }
} 
  
Check-Service –Server Server01, Server02, Server03 **



Advanced functions in V2 of PowerShell can alleviate this problem (a topic for a whole new post), but a workaround I’ve found for V1 is to use the Begin block to take certain command line parameters and pass them back into the function via the pipeline.



**Function Check-Service {
Param([string[]]$server=$null,[string]$service=”spooler” )
  BEGIN
  {
    if ($server -ne $null)
    {
      $server | Check-Service –Service $service
    }
  }
  PROCESS
  {
    if ($_ -ne $null)
    {
      $wmi=get-wmiobject win32_service -filter "name='$service'" -computername $_
      if ($wmi.state -eq “running”)  {
        write $True
      }
      else {  write $False  }
    }
  }
}**



This enables both of the above scenarios:



Get-Content servers.txt | Check-Service



Check-Service –Server Server01, Server02, Server03



<a href="http://blogs.msdn.com/powershell/archive/2009/03/01/powershell-folksonomy.aspx" target="_blank">PSMDTAG</a>:FAQ param



<a href="http://blogs.msdn.com/powershell/archive/2009/03/01/powershell-folksonomy.aspx" target="_blank">PSMDTAG</a>:FAQ pipeline



**Updated: (Missed a little recursion bug.. thanks Aleksandar!)**

