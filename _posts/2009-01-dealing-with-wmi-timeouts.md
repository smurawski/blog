---
layout: post
title: "Dealing with WMI Timeouts"
date: 2009-01-26 13:09
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 1, Systems Administration]
tags: [PowerShell, timeout, wmi]
---


There was a <a href="http://powershellcommunity.org/Forums/tabid/54/aff/1/aft/3526/afv/topic/Default.aspx" target="_blank">question</a> in the <a href="http://powershellcommunity.org/" target="_blank">PowerShellCommunity.Org</a> forums about WMI timeouts.



I did some digging and found there are several different types of timeouts that can affect your WMI queries.&#160; I’m still working through different scenarios and I’d appreciate any feedback.



The first type of timeout can occur if the machine that you are targeting does not respond to network traffic (it’s down, you have the wrong IP address, firewalled, etc..).&#160; Your WMI call will usually time out after around 20 seconds.



A partial solution to that type of timeout is to ping the machine first.&#160; If you can ping that machine, you’ve eliminated a number of potential problems.&#160; You still might have permission issues or firewall issues, but you are able to reach the machine.



The second time of timeout I’ve seen is when the connection is made, but the WMI call hangs and does not return.



A potential fix for this issue is to use the [WMISearcher] accelerator.&#160; In the options property of the resulting object, there is a ReturnImmediately property, which should be set by default to true.



$searcher = [WMISearcher]''



$searcher.Options.ReturnImmediately



The ReturnImmediately property being set to true requires that the invoked operation (the search) be done synchronously and return immediately.&#160; The enumeration of the results of the WMI call are handled after the return of this operation.



An alternative solution for this issue would be to check the Transaction timeout setting on the target machine.&#160; Run DCOMCnfg.exe and navigate to the Component Services-&gt;My Computer.&#160; Right click and choose properties.&#160; On the options tab, there is a timeout (in seconds) for DCOM transactions.&#160; The default value for this setting is 60 seconds.&#160; Fiddle with that at your own risk.&#160; Also, I haven’t had a chance to check for where that value is in the registry or see if there is a Group Policy setting for it.&#160; Setting this value should allow a failed WMI connection to timeout more quickly.



The third area in which I’ve found timeouts/hangs in WMI queries is during the enumeration of the query results.&#160; There is a property in options for the WMI class accelerator called Timeout, which sets the length of time between enumerations before timing out.&#160; The options property is hidden in the default view of PowerShell, so you will need to access it via the psbase property.&#160; 



The timeout value is a <a href="http://msdn.microsoft.com/en-us/library/system.timespan.aspx" target="_blank">System.TimeSpan</a> object.&#160; You can specify the value with a TimeSpan object, a number of ticks, or a string that can be cast to a TimeSpan.



It can be set like this:



$wmi = [wmi]''



$wmi.psbase.options.timeout ='0:0:2' #String that will be cast to a two second TimeSpan



You can also set a timeout value for the [WMISearcher] accelerator.



$searcher = [WMISearcher]''



$searcher.Options.timeout = '0:0:2'



WMI timeouts can be aggravating, hopefully one of these solutions will help mitigate your WMI woes. 

