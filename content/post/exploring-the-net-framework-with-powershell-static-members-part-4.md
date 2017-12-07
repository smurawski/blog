---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- Base Class Libraries
- General
- Introduction
- PowerShell
comments: true
date: 2009-03-01T00:00:00Z
tags: []
title: Exploring the .NET Framework with PowerShell – Static Members (Part 4)

---

In our [Exploring the .NET Framework series](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf517e4b0c945709cff39/1353512215537/?format=original), we’ve covered some terminology, creating instances of objects and calling methods.  In today’s installment, we are going to look at using static members - methods and properties.  Static methods are methods that a class (or type) make available without needing to create an instance of the class.  Similarly, static properties are properties that you can access without needing an instance of the class.
Many classes provide static members supply functionality where it doesn’t make sense to need an object. 



The <a href="http://msdn.microsoft.com/en-us/library/system.math.aspx" target="_blank">System.Math</a> class contains a couple of static properties, PI and E. 



PS C:\scripts\PowerShell&gt; [math]::E
2.71828182845905
PS C:\scripts\PowerShell&gt; [math]::PI
3.14159265358979



Constant values are not the only type of property that you’ll find.  The <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.aspx" target="_blank">System.AppDomain</a> class has a static property, <a href="http://msdn.microsoft.com/en-us/library/system.appdomain.currentdomain.aspx" target="_blank">CurrentDomain</a>, which is an AppDomain instance referring to the current application domain.



PS C:\scripts\PowerShell&gt; [AppDomain]::CurrentDomain.gettype()



IsPublic  IsSerial Name         BaseType
--------    --------    ----            --------
True     False    AppDomain System.MarshalByRefObject



System.Math also contains a number of static methods that provide a great deal of basic mathematical function.



PS C:\scripts\PowerShell&gt;[Math] | get-member –static –membertype method | select name



Name
----
Abs
Acos
Asin
Atan
Atan2
BigMul
Ceiling
Cos
Cosh
DivRem
Equals
Exp
Floor
IEEERemainder
Log
Log10
Max
Min
Pow
ReferenceEquals
Round
Sign
Sin
Sinh
Sqrt
Tan
Tanh
Truncate



I wrote a little helper function to get the static method definitions (<a href="http://poshcode.org/968" target="_blank">Get-StaticMethodDefinition</a>), which you can grab from <a href="http://poshcode.org/" target="_blank">PoshCode.org</a>.



(V2 CTP3 - We can use the trick of using the method name without parenthesis following to get additional information on each of the methods that we are interested in.)



One type of static method that I would like to highlight is the factory method.  A factory method is a common way of controlling object creation.  At its most basic level, a factory pattern is a method that creates objects of a specified type. 



An example of this is the <a href="http://msdn.microsoft.com/en-us/library/system.net.webrequest.aspx" target="_blank">System.Net.WebRequest’s</a> Create method.



PS C:\scripts\PowerShell&gt; $web = [net.webrequest]::create('[http://www.google.com](http://www.google.com)')
PS C:\scripts\PowerShell&gt; $web.gettype()



IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     HttpWebRequest                           System.Net.WebReq...



The factory method Create used the argument (‘http://www.google.com’) to determine the type of <a href="http://msdn.microsoft.com/en-us/library/system.net.webrequest.aspx" target="_blank">WebRequest</a> object to create.  In this case, it created an <a href="http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx" target="_blank">HttpWebRequest</a> object, which is a more specialized version of the WebRequest for HTTP requests. 



Let’s see how it would respond if we passed the Create method a URI with FTP.



PS C:\scripts\PowerShell&gt; $web = [net.webrequest]::create('<a href="ftp://ftp.google.com'">ftp://ftp.google.com'</a>)
PS C:\scripts\PowerShell&gt; $web.gettype()



IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    FtpWebRequest                            System.Net.WebReq...



As we can see, the WebRequest class created an <a href="http://msdn.microsoft.com/en-us/library/system.net.ftpwebrequest.aspx" target="_blank">FtpWebRequest</a> object.



Factory methods provide developers a way to more flexibly support different operations by allowing the static method to determine which object to return.



**UPDATE:  <a href="http://huddledmasses.org/" target="_self">Joel "Jaykul" Bennet</a> added a <a href="http://poshcode.org/969" target="_blank">V2 advanced function to convert static methods to functions</a> to PoShCode.org  and pointed out <a href="http://www.nivot.org/2007/08/13/CreatingFunctionsFromANETClasssStaticMethods.aspx" target="_blank">Oisin Grehan's blog post about creating functions from static methods (V1 compatible)</a>.**



I’ve been really happy to receive some of the feedback (positive and constructive criticism) on this series.  There is a lot of the <a href="http://msdn.microsoft.com/en-us/netframework/default.aspx" target="_blank">.NET Framework</a> to cover, and I’m open to suggestions as to what you would like to know.  Please post a comment on the blog or email me at Steve at UsePowerShell.Com.

