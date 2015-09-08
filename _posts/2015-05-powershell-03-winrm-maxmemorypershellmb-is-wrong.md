---
layout: post
title: "PowerShell 3 - WinRM MaxMemoryPerShellMB Is Wrong"
date: 2015-05-21 14:43
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


I was trying to test some Chef configurations on a Server 2012 image and kept hitting out of memory errors - StackOverflowExceptions, failures to load the CLR, and failures to start a shell due to limited page file.




It turns out that Windows Management Framework 3 doesn't actually use the MaxMemoryPerShellMB setting in Wsman:\localhost\Shell and uses a default 150MB value, not the 1024MB default the WSMAN configuration shows.




[There is a hotfix out, but you need to request it](https://support.microsoft.com/en-us/kb/2842230/en-us).




You can check if it is installed with 




`get-hotfix -id KB2842230`

