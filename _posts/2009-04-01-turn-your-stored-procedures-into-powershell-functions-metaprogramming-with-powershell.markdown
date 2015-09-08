---
layout: post
title: "Turn Your Stored Procedures Into PowerShell Functions - MetaProgramming With PowerShell"
date: 2009-04-08 16:38
author: steven.murawski@gmail.com
comments: true
categories: [Development, General, PowerShell, Scripts, SQL]
tags: []
---


**UPDATE: The script was moved to Google Code.  The links in the post have been updated to reflect that.  Or you can just go here... <a href="http://code.google.com/p/poshcodegen/" target="_blank">http://code.google.com/p/poshcodegen/</a>**
I’ve been working on some data conversion at work, converting records from one system to a new system.  I’ve built quite a library of SQL queries with PowerShell wrappers for dealing with data in the first system, but I don’t have the same luxury with the new system.



The new system does, however, have a nice set of stored procedures that make moving data into their application much easier.



I started writing my conversion scripts in PowerShell, since I do have to do some processing on the records to accommodate the new workflow and data layout.  I was looking at having to call almost 100 stored procedures through various parts of this process.  That is a lot of boiler plate code or referring back to the database often to check parameter names and types.  <a href="http://poshcode.org/1011" target="_blank">So, I’ve written a little PowerShell script that will take stored procedures (either as a parameter or from the pipeline) and create a function that wraps that stored procedure</a>.



The benefits of this are great with V2 CTP3 or with a more advanced editor (one that will provide tab completion on parameters).



One a wider scope, I think that this type of utility is one of PowerShell’s great strengths.  Using PowerShell for metaprogramming (<a href="http://www.tellingmachine.com/post/2008/11/Meta-Programming-with-PowerShell-and-Regular-Expressions.aspx" target="_blank">another example here on the Telling Machine blog</a>) can be a great time saver.  I spent a couple of hours working on this script, but it would have cost me much more time to handle each case individually.



Now, when I hear “metaprogramming”, my head starts to hurt a bit as I start to think about programs about programs about programs, but this isn’t that bad.  PowerShell makes this pretty easy to understand though.  To create a function dynamically, all that is needed is a string that contains text that the PowerShell runtime can evaluate (PowerShell will check for syntax errors, but not logic errors – as is the case with any script or function).



Example:



$text = 'Get-ChildItem *.ps1 | Measure-Object'



Set-Item –Path function:global:Get-PowerShellScriptCount –Value $Text



This takes advantage of the Function provider and creates a function object in the global scope with the specified name and $Text is turned into a scriptblock.  I can then call that function as needed.



Since I know PowerShell, to build dynamic functions I just have to create text that can be evaluated to do the function I need.



Here’s what <a href="http://code.google.com/p/poshcodegen/" target="_blank">my script</a> does after it runs a query against the database to get the stored procedure’s text:



1.  parses the text to get the parameter names and types
2.  using the names and types, it sets the parameters for the function (if someone wants to add some logic to make it type safe, that would rock!)
3.  builds the text to create a SqlConnection object to handle the database connection
4.  builds the text to create a SqlCommand object and sets the type of command to be a stored procedure
5.  builds the text to populate the parameters, including setting up the output parameters (if any)
6.  builds the text to run the stored procedure
7.  builds the text to put any output parameters into a PSObject as properties.
8.  create a new function with the name of the stored procedure and uses the text built in the previous steps as the scriptblock for the function.


****



**<span style="font-size: small;">What are you automating with PowerShell? </span>**



**<span style="font-size: small;">How about trying to automate some of your automation code?</span>**



<a href="http://code.google.com/p/poshcodegen/" target="_blank">New-StoredProcFunction.ps1 here.</a>



PSMDTAG:metaprogramming sql stored procedure



**UPDATED SCRIPT: Thanks to <a href="http://chadwickmiller.spaces.live.com/default.aspx" target="_blank">Chad Miller</a>**** for the idea.. instead of parsing the text of the stored procedure, the parameter information is available in the Information_Schema.Parameters**

