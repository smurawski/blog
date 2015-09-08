---
layout: post
title: "“Diff”ing Database Table Columns"
date: 2009-03-26 08:06
author: steven.murawski@gmail.com
comments: true
categories: [General, PowerShell, Scripts, SQL]
tags: []
---


Previously, [I published a script on comparing what tables two databases contained](/blog/2009/02/comparing-database-schemas).&#160; Going a bit further, I put together a script that compares the columns and what type of data they store.



<a href="http://poshcode.org/974" target="_blank">Compare-DatabaseColumns</a> has similar parameters to the <a href="http://poshcode.org/865" target="_blank">Compare-DatabaseSchema</a> script.



*   Table – One or more tables to compare columns from
*   SqlServerOne – SQL Server for the first database 
*   FirstDatabase – Name of the first database for the comparison 
*   SqlUsernameOne – SQL user name for the first database 
*   SqlPasswordOne – SQL password for the first database 
*   SqlServerTwo – SQL Server for the second database 
*   SecondDatabase – Name of the second database for comparison 
*   SqlUsernameTwo – SQL user name for the second database 
*   SqlPasswordTwo – SQL password for the second database 
*   FilePrefix – Prefix for the log file name 
*   Log – Switch parameter that saves one CSV file with the difference in the tables.&#160; If the Column switch parameter is chosen also, it will save one CSV file per table with differences in the columns 


This script can also take pipeline input, either strings or a property of “Name” or “TableName”.



You can find <a href="http://poshcode.org/974" target="_blank">this script</a> on <a href="http://poshcode.org" target="_blank">PoshCode.org</a>.

