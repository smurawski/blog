---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- Base Class Libraries
- Introduction
- PowerShell
comments: true
date: 2009-04-01T00:00:00Z
tags: []
title: Exploring the .NET Framework with PowerShell – Namespaces (Part 5)

---

One concept that is central to the understanding of the layout of classes in the [Base Class Libraries](/blog/2009/02/exploring-the-net-framework-with-powershell-terminology-part-1) is the Namespace.  Namespaces are a method of providing context for classes (and other constructs like Enums and Structs), allowing developers to group related classes and not worry about name collisions with development in other areas.
In the Base Class Libraries, the root namespace is the System namespace.  The System namespace defines a large amount of the base types, like <a href="http://msdn.microsoft.com/en-us/library/system.object.aspx" target="_blank">Object</a> (which all classes derive from), <a href="http://msdn.microsoft.com/en-us/library/system.string.aspx" target="_blank">String</a>, <a href="http://msdn.microsoft.com/en-us/library/system.datetime.aspx" target="_blank">DateTime</a>, <a href="http://msdn.microsoft.com/en-us/library/system.boolean.aspx" target="_blank">Boolean</a>, and <a href="http://msdn.microsoft.com/en-us/library/system.aspx" target="_blank">numerous others</a>.  When you specify types, <a href="http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx" target="_blank">PowerShell</a> can infer the “System” part.



Example:



>

PS C:\scripts\PowerShell&gt; $random = <a href="http://technet.microsoft.com/en-us/library/d203c4ab-2d57-4103-a04e-d7869d06931e" target="_blank">New-Object</a> –TypeName <a href="http://msdn.microsoft.com/en-us/library/system.random.aspx" target="_blank">Random</a>





is the same as



>

PS C:\scripts\PowerShell&gt; $random = <a href="http://technet.microsoft.com/en-us/library/d203c4ab-2d57-4103-a04e-d7869d06931e" target="_blank">New-Object</a> –TypeName <a href="http://msdn.microsoft.com/en-us/library/system.random.aspx" target="_blank">System.Random</a>





Namespaces are hierarchical, except in naming convention. The representation of a file in PowerShell is a great example.  A file object is represented in PowerShell as a <a href="http://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx" target="_blank">System.IO.FileInfo</a> object.  The <a href="http://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx" target="_blank">FileInfo</a> class lives in the <a href="http://msdn.microsoft.com/en-us/library/system.io.aspx" target="_blank">System.IO</a> namespace, which is a separate namespace than the <a href="http://msdn.microsoft.com/en-us/library/system.aspx" target="_blank">System</a> namespace.  There is a hierarchical impression implied, but that is solely through naming convention.



The CLR (Common Language Runtime) does not care about namespaces, only the fully qualified name of the class, so why use namespaces at all?



Namespaces are a convenience for developers, as languages like C# and VB.NET have language constructs that allow the developer to not have to use the fully qualified type name every time that is needed to be referenced.  C# provides the “<a href="http://msdn.microsoft.com/en-us/library/aa664766(VS.71).aspx" target="_blank">using</a>” syntax and VB.NET has the “<a href="http://msdn.microsoft.com/en-us/library/zt9tafza(VS.80).aspx" target="_blank">imports</a>” syntax.



How does this apply to PowerShell?



PowerShell does not currently have a “using” or “imports” syntax or language feature, but there are a couple of workarounds.  You can use variables holding the namespace and string concatenation to save some typing



>

PS C:\scripts\PowerShell&gt; $netinfo = 'System.Net.NetworkInformation'
PS C:\scripts\PowerShell&gt; $ping = New-Object "$netinfo.ping"
PS C:\scripts\PowerShell&gt; $ping.gettype()
IsPublic IsSerial Name           BaseType
-------- -------- ----                --------
True     False    Ping            System.ComponentModel.Component





This doesn’t work with type accelerators though (at least in V1)..  V2 CTP3 has some other options and <a href="http://www.nivot.org/2008/12/25/ListOfTypeAcceleratorsForPowerShellCTP3.aspx" target="_blank">Oisin Grehan has a great discussion on how to find the type accelerators and add your own.</a>



Another option would be to create a function to wrap the creation of objects for particular namespaces.



Use-Namespace is a function I came up with to create a function to wrap New-Object for specific namespaces.



The useage of Use-Namespace is
PS C:\scripts\PowerShell&gt;Use-Namespace System.IO, System.Net
PS C:\scripts\PowerShell&gt;$memorystream = New-System.IOObject MemoryStream
PS C:\scripts\PowerShell&gt;$memorystream.GetType()
PS C:\scripts\PowerShell&gt; $memorystream.gettype()
IsPublic IsSerial  Name                    BaseType
-------- --------   ----                         --------
True     True     MemoryStream      System.IO.Stream



And the function is here..



function Use-Namespace()
{
  param ([string[]]$Namespace)
  BEGIN
  {
    if ($Namespace.length -gt 0)
    {
      $Namespace | Use-Namespace
    }
  }
  PROCESS
  {
    if ($_ -ne $null)
    {
      $NS = $_
      $NSObject = "$NS" + "Object"
      $function = @"
param (`$Class, [string[]]`$ArgumentList)
if (`$ArgumentList.length -gt 0)
{
return New-Object -TypeName "$NS.`$Class" -ArgumentList `$ArgumentList
}
else
{
return New-Object -TypeName "$NS.`$Class"
}
"@
    Set-Item -Path "function:global:New-$NSObject" -Value $function -force
    Write-Debug "Created function:global:New-$NSObject "
    }
  }
}



PSMDTAG:FAQ CLR
PSMDTAG:FAQ Namespace
PSMDTAG:FAQ .NET Framework

