---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 1
- Scripts
- SQL
- Systems Administration
comments: true
date: 2009-02-01T00:00:00Z
tags: []
title: Comparing Database Schemas

---

I regularly am working with several versions of a database for an application that I manage (a live database, training database, test database, and previous version database).&#160; Occasionally, I need to know what the differences between the databases are, especially after our vendor updates my test environment or right after an update in my training or live environment.&#160; 



Since I spend a good portion of my day in PowerShell, I wrapped some system table queries in a PowerShell script and use Compare-Object to find any differences in the tables and compare the column definitions as well.&#160; The queries targets only user tables. 



Compare-DatabaseSchema.ps1 takes several parameters.



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
*   Column – Switch parametr that enables a comparison of the columns in the tables that match.&#160; Columns are compared by name and datatype. 


I still have to add some checks for the various constraints, but that will come later.



You can find <a href="http://poshcode.org/865" target="_blank">the script</a> on <a href="http://poshcode.org/" target="_blank">PoShCode.org</a>.

