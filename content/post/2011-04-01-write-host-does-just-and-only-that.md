---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 1
- PowerShell Version 2
- Scripting Games
- Tip
comments: true
date: 2011-04-01T00:00:00Z
tags:
- 2011ScriptingGames
title: Write-Host Does Just (and Only) That

---

I've seen a number of scripts that are using Write-Host to display output from their scripts.



For example:



Write-Host &quot;SomeValue = $MyValue&quot;</pre>

    
    ## So What?
    
    

    
    I'll tell you what.. If I want to do anything with that output other than look at it in the console (or perhaps a shell transcript), it gets me nothing.&#160; 
    

    
    [I'm not going to rehash why our scripts should return objects rather than text, as I covered that in a previous post](/blog/2011/04/output-optionsuse-objects). 
    

    
    ## This is Important!
    
    

    
    If you want to be able to do anything at all with your output (other than look at it, you need to output an object (remember, even text is treated as an object to PowerShell).&#160; Write-Host is quite inviting, but with a simple change to Write-Output, we'll still see the text on our console, but the resulting output can be piped or redirected to a file.
    
<pre language="powershell">Write-Output &quot;SomeValue = $MyValue&quot;</pre>

    
    ## Isn't Write-Host Just Like &quot;Echo&quot;?
    
    

    
    One of the common aliases from the DOS and CMD.exe world is &quot;echo&quot;.&#160; That's actually an alias for Write-Output!
    

    
    After we get to writing output, it'll only be a short time before you start moving to returning structured objects that the other built in commands can format, display, and output in any way necessary.
    
<pre language="powershell">$MyReportingObject = New-Object PSObject -Property @{
  Name=&quot;Scripting Games&quot;
  Value=&quot;Learning PowerShell&quot;
  Result=&quot;Being More Effective At Work&quot;
}
Write-Output $MyReportingObject

