---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- Base Class Libraries
- General
- Introduction
- PowerShell
comments: true
date: 2009-02-01T00:00:00Z
tags: []
title: Exploring the .NET Framework with PowerShell – Calling a Method (Part 2b)

---

Continuing from where [I left off last week](/blog/2009/02/exploring-the-net-framework-with-powershell-calling-a-method-part-2a), we were created an instance of the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.aspx" target="_blank">System.Net.NetworkInformation.Ping</a> class and looked at the different ways (overloads) that we can call the Send method.&#160; Now that we’ve seen how we can create Ping objects, let’s take advantage of options for creating some custom ping packets and examine the return object.



PS C:\&gt; $encoder = new-object System.Text.ASCIIEncoding



PS C:\&gt; $bytes = $encoder.Getbytes('Hello From Steve')



PS C:\&gt; $response = $ping.Send('google.com', 100, $bytes)



Let’s take a look at the response we get back.



PS C:\&gt; $response | Get-Member



&#160;&#160; TypeName: System.Net.NetworkInformation.PingReply



Name&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; MemberType&#160;&#160;&#160;&#160; Definition    <br>----&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; ----------&#160;&#160;&#160;&#160; ----------     <br>Equals&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Boolean Equals(Object obj)     <br>GetHashCode&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Int32 GetHashCode()     <br>GetType&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Type GetType()     <br>get_Address&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Net.IPAddress get_Address()     <br>get_Buffer&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Byte[] get_Buffer()     <br>get_Options&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Net.NetworkInformation.PingOptions...     <br>get_RoundtripTime Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Int64 get_RoundtripTime()     <br>get_Status&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.Net.NetworkInformation.IPStatus ge...     <br>ToString&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Method&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; System.String ToString()     <br>Address&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Property&#160;&#160;&#160;&#160;&#160;&#160; System.Net.IPAddress Address {get;}     <br>Buffer&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Property&#160;&#160;&#160;&#160;&#160;&#160; System.Byte[] Buffer {get;}     <br>Options&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Property&#160;&#160;&#160;&#160;&#160;&#160; System.Net.NetworkInformation.PingOptions...     <br>RoundtripTime&#160;&#160;&#160;&#160;&#160;&#160; Property&#160;&#160;&#160;&#160;&#160;&#160; System.Int64 RoundtripTime {get;}     <br>Status&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Property&#160;&#160;&#160;&#160;&#160;&#160; System.Net.NetworkInformation.IPStatus St...     <br>BufferSize&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; ScriptProperty System.Object BufferSize {get=$this.Buffe...     <br>OptionsString&#160;&#160;&#160;&#160;&#160;&#160;&#160; ScriptProperty System.Object OptionsString {get='TTL={0}...



Get-Member shows us that this is a <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.pingreply.aspx" target="_blank">PingReply</a> object in the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.aspx" target="_blank">System.Net.NetworkInformation</a> namespace.



Looking at the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.pingreply.aspx" target="_blank">PingReply</a> object that was returned, we have several methods that look strangely similar to the properties, but with a “get_” prefixed to it.&#160; When you see a method that matches a Property, but has “get_” or “set_”, that is a method exposed by the <a href="http://msdn.microsoft.com/en-us/netframework/default.aspx" target="_blank">.NET Framework</a> that provides the functionality for the properties.&#160; When you access the Buffer property, you are actually calling the get_Buffer method.&#160; Properties are, for the most part, easier to work with and you can effectively ignore those methods.&#160; Properties also contain a clue in their description, if you look at the end of the Buffer property’s description, you see “{get;}” which indicates that there is a way to retrieve that property.&#160; Properties can also show “{set;}” which indicates that you can update that property.



In <a href="http://www.microsoft.com/DOWNLOADS/details.aspx?FamilyID=c913aeab-d7b4-4bb1-a958-ee6d7fe307bc&amp;displaylang=en" target="_blank">PowerShell V2 CTP3</a> those “getters” and “setters” (as those methods are commonly called) are hidden by default.&#160; If you are running CTP3 and want to see the “getters” and “setters”, you can call Get-Member with the Force parameter.



PS C:\&gt; $response | Get-Member –Force



Back to the properties of our return object.&#160; The properties names are pretty self-explanatory, but to confirm we got back the “special” packet we crafted, let’s take a look at the Buffer property and convert that back to text.



PS C:\&gt; $encoder.GetString($response.buffer)    <br>Hello From Steve



Now for the meat of the response, the RoundtripTime and Status.&#160; I commonly use the Roundtrip time to watch for latency on my network, and since it is exposed so nicely, it is easy to log.



PS C:\&gt; $response.RoundtripTime    <br>74



And of course, we are interested in the Status.



PS C:\&gt; $response.Status    <br>Success



Now, we can see from the Get-Member return above, that Status is not just a string that says, “Success”, it is a <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ipstatus.aspx" target="_blank">IPStatus</a> enumeration (more on enumerations coming up, but long story short, there are a number of predefined values that an <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ipstatus.aspx" target="_blank">IPStatus</a> object can hold)(also found in the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.aspx" target="_blank">System.Net.NetworkInformation</a> namespace).&#160; 



I’m normally checking the IPStatus for a Success value like



PS C:\&gt; $success = [System.Net.NetworkInformation.IPStatus]::Success



PS C:\&gt; $response.status -eq $success    <br>True



I could compare the string results, but then I’m stepping back to parsing text results.&#160; By using the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ipstatus.aspx" target="_blank">IPStatus</a> enumeration values, I can get be reasonably sure that my value are correct and I don’t have any text matching issues.



In the next post in this series, we’ll take a look at a special type of method, the constructor.

