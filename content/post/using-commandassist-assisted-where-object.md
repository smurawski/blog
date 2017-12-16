---
author: steven.murawski@gmail.com
categories:
- CommandAssist
- Modules
comments: true
date: 2011-08-01T00:00:00Z
tags: []
title: Using CommandAssist - Assisted Where-Object

---

To use <a href="http://download.usepowershell.com/commandassist.zip" target="_blank">CommandAssist</a>, the first step you will need to take is to have <a href="http://showui.codeplex.com/" target="_blank">ShowUI</a> (at least 1.1) installed and run <a href="http://technet.microsoft.com/en-us/library/dd315276.aspx" target="_blank">PowerShell in STA mode</a>.



Then, you can import CommandAssist.&#160; After importing CommandAssist, you can take advantage of the scriptblock builder built in to Where-Object 



Get-ChildItem | Where-Object -assist



When you hit enter, you will see a screen like the below.



<a href="http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c1e4b0c945709cfb80/1312786472000/?format=original">![image](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c1e4b0c945709cfb83/1312786473000/?format=original "image")</a>



At the top of the screen is a count of the number of objects coming into Where-Object.&#160; Underneath that is the textbox for the filter scriptblock.&#160; After you start adding information to the scriptblock, it will turn green for valid scriptblocks and red for invalid scriptblocks.&#160; Underneath that is a count of how many objects will pass through the pipeline after applying the filter.



Two list boxes towards the bottom contain a list of properties available from the incoming objects, as well as the various comparison operators in PowerShell.&#160; You can drag and drop those items into the scriptblock builder.



Once you are happy with the resulting scriptblock, you can hit OK and the selected filter will be applied to the objects in the pipeline and pass the remaining objects to the next step in the pipeline.

