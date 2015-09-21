---
layout: post
title: "Stop Crippling WinRM"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

## Know what you are configuring

Most folks who use Chef or other projects that do lots of stuff over WinRM have to customize the settings.  This can be to allow Basic authentication, increase then number of default shells per user, or the most commonly to increase the maximum memory a shell can use.

Everyone has their pet script for doing this, usually based on bumping up limits that were the default on Server 2008 R2.  However, in Server 2012 (and Windows Management Framework 3), many of those limits were increased rathered dramatically.  Now, if are using the same old script you used to set up Server 2008 R2 to set up 2012 or 2012 R2 - you are actively harming your ability to do work.

Most older scripts for configuring WinRM doubled the 150MB MaxMemoryPerShellMB setting based on those Server 2008 R2 defaults..  Server 2012 (and Windows Management Framework 3) bumped the default limit for MaxMemoryPerShellMB to 1024 ([though there is a bug]({{ site.baseurl }}/2015/05/powershell-3-winrm-maxmemorypershellmb-is-wrong/index.html)).  Blindly using some dated configuration script is actively hurting you.
