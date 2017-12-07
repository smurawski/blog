---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- Base Class Libraries
- Introduction
- PowerShell
- PowerShell Version 1
comments: true
date: 2009-03-01T00:00:00Z
tags: []
title: Exploring the .NET Framework with PowerShell – Constructors (Part 3)

---

In part 2([a](/blog/2009/02/exploring-the-net-framework-with-powershell-calling-a-method-part-2a) &amp; [b](/blog/2009/02/exploring-the-net-framework-with-powershell-calling-a-method-part-2b)) of this series, we talked about methods and looked at ways to view their overloads, or ways to call them.&#160; We also looked at the objects returned from a method call.&#160; In this post, we are going to explore a special kind of method called the constructor.



A constructor is a method whose job is to create the object that you want to work with.&#160; When I created the Ping object



PS C:\scripts\PowerShell&gt; $ping = New-Object System.Net.NetworkInformation.Ping



the <a href="http://technet.microsoft.com/en-us/library/d203c4ab-2d57-4103-a04e-d7869d06931e" target="_blank">New-Object</a> cmdlet calls the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.ping.aspx" target="_blank">constructor for Ping</a>.



Like other methods, constructors can require parameters and can have multiple overloads.&#160; 



Let’s look at <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx#" target="_blank">System.Net.Mail.MailMessage</a>, which is a object that represents an email message.



The <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.mailmessage.aspx" target="_blank">MailMessage constructor</a> contains several overloads.&#160; The first method listed is MailMessage(), which is the default constructor.&#160; This method does not require any arguments and returns an empty MailMessage object.&#160; In <a href="http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx" target="_blank">PowerShell</a>, we would call this method via <a href="http://technet.microsoft.com/en-us/library/d203c4ab-2d57-4103-a04e-d7869d06931e" target="_blank">New-Object</a>, just like with Ping class.



MailMessage()



 PS C:\scripts\PowerShell&gt; $message = New-Object System.Net.Mail.MailMessage



The first overload requires two <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.mailaddress.aspx" target="_blank">MailAddress</a> objects, the first of which is the “from” address and the second is the “to” address.&#160; 



MailMessage(   <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; MailAddress from,    <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; MailAddress to,    <br>)



Creating an object in PowerShell from a constructor that requires parameters can be done in two ways.&#160; The first is to use the method call notation.



 PS C:\scripts\PowerShell&gt; $message = New-Object System.Net.Mail.MailMessage($MailFrom, $MailTo)



The second is to use the ArgumentList parameter.



 PS C:\scripts\PowerShell&gt; $message = New-Object System.Net.Mail.MailMessage –ArgumentList $MailFrom, $MailTo



If you happen to have <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.mailaddress.aspx" target="_blank">MailAddress</a> objects floating around, this might work for you.&#160; A more common scenario would be to use the next overload, which takes two strings.



MailMessage(   <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; String from,    <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; String to,    <br>)



This overload requires two strings, one for the “to” and one for the “from” address.&#160; 



The final overload for MailMessage is



MailMessage(   <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; String from,    <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; String to,    <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; String subject,    <br>&#160;&#160;&#160;&#160;&#160;&#160;&#160; String body,    <br>)



This overload allows you to pass in four strings and get back basic MailMessage object that is ready to be sent.



Wouldn’t it be great to have a function, where we could provide a type name and get a listing of all the constructors, right from the command line (not needing to go to the MSDN documentation every time).&#160; Jeffrey Snover has beaten me to the punch and provided a function to find the constructors, appropriately called <a href="http://blogs.msdn.com/powershell/archive/2008/09/01/get-constructor-fun.aspx" target="_blank">Get-Constructor</a>.&#160; Add that function to the your toolbox, and exploring the .NET Framework becomes a lot easier.



Next up, we’ll look at static methods.&#160; Static methods are methods that can be called without having to create a instance of an object.



One particular type of static method that we’ll dig in to is the factory method, which is another way to create instances of objects.

