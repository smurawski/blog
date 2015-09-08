---
layout: post
title: "Exploring the .NET Framework with PowerShell – Calling a Method (Part 2a)"
date: 2009-02-11 22:06
author: steven.murawski@gmail.com
comments: true
categories: [-NET Framework, Base Class Libraries, Introduction, PowerShell]
tags: []
---


[Last week, I defined a number of terms](/blog/2009/02/exploring-the-net-framework-with-powershell-terminology-part-1) that we’ll be exposed to as we delve into how and why <a href="http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx" target="_blank">PowerShell</a> is an object oriented shell and how to use it to explore .NET Framework which it is built upon.
Now, let’s take a look at how some of these terms surface themselves in PowerShell.



One of the most common tasks you might encounter is needing to ping a computer.  There is ping.exe, which works and has been a common guest in my console sessions for years.  You can also get ping information via WMI which provides you with some interesting information.



PS C:\scripts\PowerShell&gt; Get-WmiObject Win32_PingStatus -filter "Address='google.com'"



My preference is to use a class from the .NET Framework called <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.aspx" target="_blank">Ping</a>.  <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.aspx" target="_blank">Ping</a> lives in the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.aspx" target="_blank">System.Net.NetworkInformation</a> namespace.



To access this in PowerShell, I’ll create an instance of the class and refer to it with a variable, $ping.



PS C:\scripts\PowerShell&gt; $ping = New-Object System.Net.NetworkInformation.Ping



By typing the method name without the parenthesis immediately after it, PowerShell will give us information about the method.



PS C:\scripts\PowerShell&gt; $ping.send



MemberType          : Method
OverloadDefinitions : {System.Net.NetworkInformation.PingReply Send(String ho
                      stNameOrAddress), System.Net.NetworkInformation.PingRep
                      ly Send(String hostNameOrAddress, Int32 timeout), Syste
                      m.Net.NetworkInformation.PingReply Send(IPAddress addre
                      ss), System.Net.NetworkInformation.PingReply Send(IPAdd
                      ress address, Int32 timeout)...}
TypeNameOfValue     : System.Management.Automation.PSMethod
Value               : System.Net.NetworkInformation.PingReply Send(String hos
                      tNameOrAddress), System.Net.NetworkInformation.PingRepl
                      y Send(String hostNameOrAddress, Int32 timeout), System
                      .Net.NetworkInformation.PingReply Send(IPAddress addres
                      s), System.Net.NetworkInformation.PingReply Send(IPAddr
                      ess address, Int32 timeout), System.Net.NetworkInformat
                      ion.PingReply Send(String hostNameOrAddress, Int32 time
                      out, Byte[] buffer), System.Net.NetworkInformation.Ping
                      Reply Send(IPAddress address, Int32 timeout, Byte[] buf
                      fer), System.Net.NetworkInformation.PingReply Send(Stri
                      ng hostNameOrAddress, Int32 timeout, Byte[] buffer, Pin
                      gOptions options), System.Net.NetworkInformation.PingRe
                      ply Send(IPAddress address, Int32 timeout, Byte[] buffe
                      r, PingOptions options)
Name                : Send
IsInstance          : True



What I am most interested in is the Overload Definitions, which is the explanation of how I can use this method.



PS C:\scripts\PowerShell&gt; $ping.send.overloaddefinitions



System.Net.NetworkInformation.PingReply Send(String hostNameOrAddress)
System.Net.NetworkInformation.PingReply Send(String hostNameOrAddress, Int32 timeout)
System.Net.NetworkInformation.PingReply Send(IPAddress address)
System.Net.NetworkInformation.PingReply Send(IPAddress address, Int32 timeout)
System.Net.NetworkInformation.PingReply Send(String hostNameOrAddress, Int32 timeout, Byte[] buffer)
System.Net.NetworkInformation.PingReply Send(IPAddress address, Int32 timeout, Byte[] buffer)
System.Net.NetworkInformation.PingReply Send(String hostNameOrAddress, Int32 timeout, Byte[] buffer, PingOptions options)
System.Net.NetworkInformation.PingReply Send(IPAddress address, Int32 timeout, Byte[] buffer, PingOptions options)



As we can see from the above output $ping.<a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.send.aspx" target="_blank">Send</a> has several different ways that we can call it and it also shows the object that we will get in return, an object of the type <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.pingreply.aspx" target="_blank">System.Net.NetworkInformation.PingReply</a>.



The first overload for $ping.<a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.send.aspx" target="_blank">Send</a> is to supply it an object of the type <a href="http://msdn.microsoft.com/en-us/library/system.string.aspx" target="_blank">String</a> (remember, in PowerShell, even text is an object) which represents the hostname or IP address of the host we wish to ping.



PS C:\scripts\PowerShell&gt; $ping.send(‘google.com’)



Status        : Success
Address       : 74.125.45.100
RoundtripTime : 29 ms
BufferSize    : 32
Options       : TTL=243, DontFragment=False



Here, we called $ping.<a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.send.aspx" target="_blank">Send</a> with a argument of the String ‘google.com’ and received back an object of the type PingReply.  We can see the textual representation of it’s properties.  In my next post, we’ll dig into the return object a bit more and see how we can use its properties to our advantage.



We still have a number of other ways that we can call $ping.<a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.send.aspx" target="_blank">Send</a>.  Our second option is to provide a <a href="http://msdn.microsoft.com/en-us/library/system.string.aspx" target="_blank">String</a> describing the hostname or IP address (as before), but also allows us to specify a timeout by providing an <a href="http://msdn.microsoft.com/en-us/library/system.int32.aspx" target="_blank">Int32</a>.  (Just a quick note, if you specify a really low timeout value, the reply still may be received.  It is more of an option to provide a longer than normal, five seconds, timeout period.)



We also see overloads that take an object of the <a href="http://msdn.microsoft.com/en-us/library/system.net.ipaddress.aspx" target="_blank">IPAddress</a> type to describe the target of the Ping.



Now, things get a bit more interesting.  The next argument we see is of the type <a href="http://msdn.microsoft.com/en-us/library/system.byte(VS.80).aspx" target="_blank">Byte</a>[], which means an <a href="http://msdn.microsoft.com/en-us/library/system.array.aspx" target="_blank">Array</a> of <a href="http://msdn.microsoft.com/en-us/library/system.byte(VS.80).aspx" target="_blank">Byte</a> objects.  The quick explanation of an <a href="http://msdn.microsoft.com/en-us/library/system.array.aspx" target="_blank">Array</a> is a collection of objects (not 100 percent accurate, but sufficient for right now).  This allows us to specify exactly what data will be sent with the ICMP echo message.



The final argument is to supply an object of the <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.pingoptions.aspx" target="_blank">PingOptions</a> type.  This allows us to specify an <a href="http://msdn.microsoft.com/en-us/library/system.int32.aspx" target="_blank">Int32</a> as the TTL (Time To Live) and a <a href="http://msdn.microsoft.com/en-us/library/system.boolean.aspx" target="_blank">Boolean</a> as Don’t Fragment option.



I’m starting to run a bit long here, so I’m going to continue walking through this example in my next post.  We’ll create an object of type <a href="http://msdn.microsoft.com/en-us/library/system.net.networkinformation.ping.aspx" target="_blank">Ping</a> with some custom options and then dig in to the object that is returned.

