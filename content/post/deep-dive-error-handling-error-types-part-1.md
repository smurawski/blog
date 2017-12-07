---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- Base Class Libraries
- Deep Dive
- Error Handling
- PowerShell Version 1
- PowerShell Version 2
comments: true
date: 2009-07-01T00:00:00Z
tags: []
title: 'Deep Dive: Error Handling – Error Types (part 1)'

---

I started looking a little deeper at error handling in PowerShell after <a href="http://stackoverflow.com/questions/1142211/try-catch-does-not-seem-to-have-an-effect" target="_blank">this StackOverflow question</a>.
PowerShell has two kinds of errors – terminating errors and non-terminating errors.



Terminating errors are the errors that can stop command execution cold.  Non-terminating errors provided an additional challenge, as you need to be notified of failed operations and continue with pipeline operations.  To deal with this issue and to provide additional output options, PowerShell employs the concept of streams.  There are three additional streams (other than the primary pipeline stream) available in PowerShell – Verbose, Warning, and Error (and .  We’ll be concerning ourselves with the output from the Error stream.



There are some <a href="http://technet.microsoft.com/en-us/library/dd347675.aspx" target="_blank">automatic variables</a> in the shell that deal with errors as well.  $ErrorActionPreference is a variable that describes how PowerShell will treat non-terminating errors.  $ErrorActionPreference has four options: “Continue” (the default), “SilentlyContinue”, “Inquire”, and “Stop”.  $error is an array of all the errors that have occurred in the current session up to the number specified in $MaximumErrorCount.  $? is a boolean value that is $true if the previous operation succeeded and $false if it did not.



PowerShell does have some built in flexibility to turn non-terminating errors into terminating errors.  Based on our setting of $ErrorActionPreference, PowerShell will treat a non-terminating error differently.  If $ErrorActionPreference is set to '”Stop”, PowerShell will treat the errors from that scope and all sub scopes (unless explicitly overridden) as terminating errors.  If “Inquire” is chosen as the $ErrorActionPreference, then PowerShell will prompt the user at the console upon each error to ask whether it should continue (treat as non-terminating) or halt (treat as terminating), as well as providing an option to drop into a nested prompt.



$ErrorActionPreference is a global preference that can be set, but the PowerShell runtime also allows more granular control by providing <a href="http://technet.microsoft.com/en-us/library/dd315352.aspx" target="_blank">common parameters</a> for -ErrorAction and -ErrorVariable (shorthand -EA and –EV) which allow you to determine per cmdlet (and in V2 per advanced function) how errors should be handled.  The –ErrorAction parameter takes the same options as $ErrorActionPreference.



So what happens when you encounter an error?



If you hit a terminating error, your script, function, or command will stop.  The error will be logged to the $error variable and written to the error stream.  The $? will be set to false.  You can use the <a href="http://technet.microsoft.com/en-us/library/dd347548.aspx" target="_blank">trap construct</a> (or the <a href="http://technet.microsoft.com/en-us/library/dd315350.aspx" target="_blank">try/catch/finally construct</a> in V2) to deal with terminating errors and we’ll cover that further in the fourth post in this series.



If you hit a non-terminating error, the result will depend on the $ErrorActionPreference in the scope or the –ErrorAction parameter.  If the $ErrorActionPreference or –ErrorAction parameter is set to ‘Continue’, the error will be written to the error stream, added to the $error array, and the $? will be set to $false.  ‘SilentlyContinue’ will not write an error to the Error stream, but $error and $? are updated.  Finally, ‘Inquire’ provides you an option at each error as to whether to treat it as terminating or non-terminating.  If treated as a non-terminating error, the error will be treated like the $ErrorActionPreference or –ErrorAction parameter is ‘Continue’.



Now, I’ve mentioned that some terminating and non-terminating errors both write to the Error stream, so where does that actually get reported back to the user?  If an error has been written to the error stream, it will be written to the output stream (which may surface differently based on the PowerShell host) separately from the pipeline output unless it is redirected.  This means that the exception objects, which are how the errors are represented, are not saved to a variable if you are assigning the pipeline output to a variable.



For example:



$results = Get-WMIObject Win32_Bios –ComputerName localhost, ComputerThatDoesNotExist
Get-WmiObject : The RPC server is unavailable. (Exception from HRESULT: 0x800706BA)
At line:1 char:22
+ $results = Get-WmiObject  &lt;&lt;&lt;&lt; Win32_Bios -ComputerName localhost, ComputerThatDoesNotExist



creates this output to the screen, but $results will only hold the results of the successful WMI query.  This is where the –ErrorVariable comes in.  You can specify a variable to hold the exceptions generated by a particular cmdlet (or in V2 advanced function).  Specifying a variable does not remove an item from being written to the error stream and displayed as output.



In a console session, you can redirect the error stream with the redirection operators ( 2&gt; or 2&gt;&gt;) or you can merge the error stream with the output stream (2&gt;&amp;1) and write the output to a file, the pipeline, or assign it to a variable.



Up next:



*   Error Objects
*   Tracing Errors
*   Trapping Errors
*   Reporting Errors


Other great <a href="http://support.microsoft.com/kb/968929" target="_blank">PowerShell</a> Error Handling posts:



*   [http://huddledmasses.org/trap-exception-in-powershell/](http://huddledmasses.org/trap-exception-in-powershell/)
*   [http://blogs.msdn.com/powershell/archive/2006/11/03/erroraction-and-errorvariable.aspx](http://blogs.msdn.com/powershell/archive/2006/11/03/erroraction-and-errorvariable.aspx)
*   [http://tfl09.blogspot.com/2009/01/error-handling-with-powershell.html](http://tfl09.blogspot.com/2009/01/error-handling-with-powershell.html)
*   [http://weblogs.asp.net/adweigert/archive/2007/10/10/powershell-try-catch-finally-comes-to-life.aspx](http://weblogs.asp.net/adweigert/archive/2007/10/10/powershell-try-catch-finally-comes-to-life.aspx)
*   [http://powershell.com/cs/blogs/ebook/archive/2009/03/30/chapter-11-finding-and-avoiding-errors.aspx](http://powershell.com/cs/blogs/ebook/archive/2009/03/30/chapter-11-finding-and-avoiding-errors.aspx)
