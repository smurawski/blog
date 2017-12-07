---
author: steven.murawski@gmail.com
categories:
- Crystal Reports
- PowerShell
- PowerShell Version 2
- Scripts
comments: true
date: 2009-11-01T00:00:00Z
tags: []
title: So Easy, I Could Kick Myself

---

I’m updating Crystal Reports and trying to determine which reports might have been affected by some schema changes or functional changes in how the data was being stored. 



The problem I’ve had is that when there are a large number of reports, it is very time consuming to open each one, look at it, and see if it contains any affected tables or views.



I’ve had to deal with this in my previous role as well.  After feeling the pain a few times, I turned my intern loose on the problem and shelved the problem as “just another pain in dealing with Crystal Reports”.



Now, I’m back dealing with Crystal Reports more frequently and in the position to have to possibly update around 30 or 40 reports that were written before I started.



I’ve recently had a bit of exposure to the object model for the <a href="http://msdn.microsoft.com/en-us/library/bb944221.aspx" target="_blank">.NET API for Crystal Reports</a> and thought maybe I could leverage that through <a href="http://msdn.microsoft.com/en-us/library/ms714418(VS.85).aspx" target="_blank">PowerShell</a> and whip together a quick script to help me list out the tables in each report.



It turned out to be painfully easy… 



<span style="color: #008080">[reflection.assembly]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">LoadWithPartialName</span><span style="color: #000000">(</span><span style="color: #8b0000">'CrystalDecisions.Shared'</span><span style="color: #000000">)</span><span style="color: #008080">[reflection.assembly]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">LoadWithPartialName</span><span style="color: #000000">(</span><span style="color: #8b0000">'CrystalDecisions.CrystalReports.Engine'</span><span style="color: #000000">)</span>
<span style="color: #ff4500">$report</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span> <span style="color: #8a2be2">CrystalDecisions.CrystalReports.Engine.ReportDocument</span>
<span style="color: #ff4500">$report</span><span style="color: #a9a9a9">.</span><span style="color: #000000">load</span><span style="color: #000000">(</span><span style="color: #ff4500">$pathToScript</span><span style="color: #000000">)</span>
<span style="color: #ff4500">$report</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Database</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Tables</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Select-Object</span> <span style="color: #000080">-expand</span> <span style="color: #8a2be2">Name</span>
<span style="color: #ff4500">$report</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Dispose</span><span style="color: #000000">(</span><span style="color: #000000">)</span>



 



After I got the basics, I poked around and updated the script further (<a href="http://poshcode.org/1471" target="_blank">and posted it on PoshCode</a>).



The full script also accesses the first level of subreports and retrieves their tables as well.



NOTE: <a href="http://wiki.sdn.sap.com/wiki/pages/viewpage.action?pageId=56787567" target="_blank">Requires either the Crystal Report Runtime (Visual Studio 2008) </a> or Visual Studio to be installed.



**[DOWNLOAD UPDATED SCRIPT ](http://download.usepowershell.com/get-crystalreporttable.ps1)**



**[DOWNLOAD VERSION FOR POWERSHELL V1 ](http://download.usepowershell.com/Get-CrystalReportTableForV1.ps1)**

