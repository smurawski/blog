---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- Base Class Libraries
- Introduction
- PowerShell
comments: true
date: 2009-02-01T00:00:00Z
tags: []
title: Exploring the .NET Framework With PowerShell – Terminology (Part 1)

---

PowerShell opens up the <a href="http://msdn.microsoft.com/en-us/netframework/default.aspx" target="_blank">.NET Framework</a> for a large new group of consumers, IT Pro’s (or systems administrators).  Microsoft developers have been working for years with the .NET Framework and now that rich sea of resources is available from our command line.
One of the first hurdles that many IT Pro’s come across when trying to see where the <a href="http://msdn.microsoft.com/en-us/netframework/default.aspx" target="_blank">.NET Framework</a> fits into their workflow is the terminology. 



Before we start digging into the guts of the <a href="http://msdn.microsoft.com/en-us/netframework/default.aspx" target="_blank">.NET Framework</a>, let’s cover some definitions:



*   **.NET Framework** – The .NET Framework is a software framework that contains the Common Language Runtime and  Base Class Libraries that provide much of the functionality.  There are several versions of the .NET Framework.  PowerShell is compatible with versions 2.0, 3.0, and 3.5. 
*   **Common Language Runtime** (**CLR**) – The Common Language Runtime is the execution environment for .NET applications.  The CLR provides memory management, exception handling, thread management, and security infrastructure.  The CLR is the engine that runs .NET applications.
*   **Base Class Libraries** (**BCL**) – The Base Class Libraries are sets of common functions accessible by any .NET language, including PowerShell.  The BCL is organized into namespaces based on functionality.  For example, the Webclient class is in the System.Net namespace.  The BCL includes functions for working with files, networking, database interaction, graphics, and much more.
*   **Namespace** – A namespace allows CLR functions to be organized and distinguished.  Using a namespace helps prevent naming conflicts without having to have extra long names.  For example, the class System.Net.NetworkInformation.Ping is in the System.Net.NetworkInformation namespace.
*   **Class** – A class is an abstract definition of some functionality.  The class is the definition that the CLR uses when objects are created.  Classes are also referred to as **Types**.  Classes can also have static methods, which are functions that can be used without having to create an instance of the class.
*   **Object** – An object is the **instance** of a class.  An object is a distinct creation in memory that follows the blueprint described in the class.  For example, System.Net.WebClient is a class.  To create an instance of that class in PowerShell, I would: $webclient = New-Object System.Net.WebClient.  The result would be an object of the type System.Net.Webclient which is pointed to by the variable $webclient.
*   **Property** – A property is data that is associated with an object or class.  Properties themselves are objects.  In our example, the System.Net.Webclient object referred to by $webclient has a property Credentials, which is an object of the type System.Net.NetworkCredentials.
*   **Method** – Methods are actions that the object (or type in the case of static methods) can perform.  Methods can have a return type and can require different sets of parameters, allowing you different ways to call them.
*   **Event** – Events are notices that an object can expose, allowing others to run code in response to a change of state of some sort.
*   **Exception** – Exceptions are the error handling mechanism in the CLR.  Exceptions are strongly typed, so you can handle expected errors in your code differently depending on the problem.


Now that we’ve got some definitions covered, we’ll start using PowerShell to expose the capabilities that the framework provides us and dig further into these definitions.

