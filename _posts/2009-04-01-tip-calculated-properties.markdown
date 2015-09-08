---
layout: post
title: "Tip: Calculated Properties"
date: 2009-04-22 05:36
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, Tip]
tags: []
---


I really enjoy [Jeffery Hicks's](http://blog.sapien.com/) [Prof. PowerShell column](http://mcpmag.com/columns/columnist.asp?columnistsid=81).  In two of his recent columns (<a href="http://mcpmag.com/columns/article.asp?editorialsid=3074" target="_blank">Perfect Properties</a> and <a href="http://mcpmag.com/columns/article.asp?editorialsid=3079" target="_blank">Label Me Perfect</a>), Jeffery goes in to "calculated properties".  
A calculated property is a hashtable passed to the Select-Object or Format-* cmdlets as one of the properties to return.



Here is an example from <a href="http://mcpmag.com/columns/article.asp?editorialsid=3079" target="_self">Label Me Perfect</a>:



>

PS C:\&gt; get-wmiobject Win32_logicaldisk -filter "drivetype=3" `
-computer  "mycompany-dc01" | Format-table  DeviceID,VolumeName,`
@{label="Size(MB)";Expression={$_.Size/1mb -as  [int]}},`
@{label="Free(MB)";Expression={$_.FreeSpace/1MB -as  [int]}},`
@{label="PercentFree";Expression={
"{0:P2}" -f  (($_.FreeSpace/1MB)/($_.Size/1MB))}} -autosize





That is a whopper to type at the command line or to try to read in a script.



In order to increase readability and resuse, you can assign those hash tables to a variable and use that variable as one of the parameters to Select-Object or one of the Format-* cmdlets.



>

PS C:\&gt; $Size = @{label="Size(MB)";Expression={$_.Size/1mb -as [int]}}
PS C:\&gt; $FreeMB = @{label="Free(MB)";Expression={$_.FreeSpace/1MB -as [int]}}
PS C:\&gt; $PercentFree = @{label="PercentFree";Expression={"{0:P2}" -f (($_.FreeSpace/1MB)/($_.Size/1MB))}}



PS C:\&gt; get-wmiobject Win32_logicaldisk -filter "drivetype=3" `
-computer "mycompany-dc01" | Format-table DeviceID,VolumeName, `
$Size, $FreeMB, $PercentFree -autosize





This makes it a bit easier to read, and also makes those hash tables available for further use in additional statements without needing to retype them.



 



PSMDTAG: FAQ: Calculated Properties

