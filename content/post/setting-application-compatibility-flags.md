---
author: steven.murawski@gmail.com
categories:
- PowerShell Version 2
- Scripts
comments: true
date: 2011-02-01T00:00:00Z
tags: []
title: Setting Application Compatibility Flags

---

There was a post on the PowerShell Facebook group today asking how to set the “Run As Administrator” checkbox on the Compatibility tab of the properties for an application. 



The compatibility settings are just registry entries, either under the user’s account or under the local machine.



(You can find them under HKCU:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers or



HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers)



Compatibility settings reference the path of the executable as the value and store the app compat flags in the data for the registry key.



<a href="http://poshcode.org/2494" target="_blank">I’ve written a quick PowerShell script to automate the setting of these compatibility flags and posted it on PoshCode</a>.

