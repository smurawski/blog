---
layout: post
title: "Creating an Assisted Module with CommandAssist"
date: 2011-08-13 09:06
author: steven.murawski@gmail.com
comments: true
categories: [CommandAssist, Modules, Scripts]
tags: []
---


The [CommandAssist](/blog/2011/08/announcing-commandassist) module allows you to easily create proxies functions for every command in a module (or snapin).&#160;&#160; 



There is a handy command to generate these proxy modules included with the CommandAssist module.&#160; 



<a href="http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c1e4b0c945709cfb86/1313226370000/?format=original">![image](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c1e4b0c945709cfb89/1313226371000/?format=original "image")</a>



### Building an Assisted Module




The easiest way to explain what happens is to walk through the usage of New-AssistedModule.&#160; To that end, we'll create a proxy of a module that I've been use regularly - the ActiveDirectory module.



**NOTE:** You will need to have <a href="http://www.showui.com" target="_blank">ShowUI</a> available in your module path (you can check with Get-Module <a href="http://www.showui.com" target="_blank">ShowUI</a>).&#160; You do not need to have it loaded into your session, CommandAssist will take care of that.



First we'll load the CommandAssist module:



Import-Module CommandAssist</pre>

    
    Second we will load the module we want to create a proxy for:
    
<pre language="powershell">Import-Module ActiveDirectory</pre>

    
    **NOTE: **This only has to be done the first time to create the proxy.&#160; Afterward, the proxy module will load the real module in the background.
    

    
    Third we will use New-AssistedModule to create our proxy module.
    
<pre language="powershell">$NewModulePath = c:\users\smurawski\Documents\WindowsPowerShell\Modules\AssistedAD
Get-Module ActiveDirectory | 
    New-AssistedModule -ModulePath $NewModulePath



<a href="http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c1e4b0c945709cfb8c/1313226377000/?format=original">![image](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfb8f/1313226379000/?format=original "image")</a>



As noted in the screenshots, the module created has proxies for each command that add the -assist parameter.&#160; There also is a module file that will load all of the proxies, as well as the source module (in this example, AssistedAD.psm1).



The next post will review how the -Assist parameter works.



**NOTE: **The proxy module will load the real module in the background (so if you distribute it, the end user will need the source module or snapin as well).

