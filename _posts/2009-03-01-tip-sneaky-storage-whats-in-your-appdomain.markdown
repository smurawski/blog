---
layout: post
title: "Tip: Sneaky Storage – What’s in your AppDomain?"
date: 2009-03-24 08:49
author: steven.murawski@gmail.com
comments: true
categories: [-NET Framework, Base Class Libraries, PowerShell]
tags: []
---


Since PowerShell is built on .NET, there is a <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.aspx" target="_blank">AppDomain</a> (I’ll go into more detail in a later post) which has a lot of information about the .NET environment (what assemblies are loaded, etc..).



One feature that the AppDomain has is to store globally accessible name/value pairs.&#160; Normally, variables in PowerShell should handle most of your “in-process” storage needs, but for the times that they don’t, you have your AppDomain.



To access the AppDomain object, you can use the static property <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.currentdomain.aspx" target="_blank">CurrentDomain</a> on the <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.aspx" target="_blank">System.AppDomain</a> class. 



**(NOTE: Though I usually refer use the fully qualified namespace, PowerShell does allow you to skip the “System” portion of the namespace.&#160; I’m using the full namespace for clarity on the blog, but when typing on the command line, it is easier to skip that.)**



PS C:\&gt; [system.appdomain]::CurrentDomain



To save a name/value pair to your AppDomain, you can use the <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.setdata.aspx" target="_blank">SetData</a> method.



PS C:\&gt; [system.appdomain]::CurrentDomain.SetData('ThisIsMyNameOrKey', ('This','Can','Be','Any','Object','Or','Collection') )



Any object or collection can be saved in the AppDomain, and you are not limited to just one.



Accessing those values is just as easy, using the <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.getdata.aspx" target="_blank">GetData</a> method.



PS C:\&gt; [system.appdomain]::CurrentDomain.GetData('ThisIsMyNameOrKey')   <br>This    <br>Can    <br>Be    <br>Any    <br>Object    <br>Or    <br>Collection



And there we have our collection back.



[PSMDTAG](http://blogs.msdn.com/powershell/archive/2009/03/01/powershell-folksonomy.aspx):.Net AppDomain

