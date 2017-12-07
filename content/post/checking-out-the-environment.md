---
author: steven.murawski@gmail.com
categories:
- PowerShell
- PowerShell Version 2
- Scripting Games
- Tip
comments: true
date: 2011-04-01T00:00:00Z
tags:
- 2011ScriptingGames
title: Checking Out The Environment

---

If you've worked with cmd.exe or other shell environments, you might be familiar with environmental variables.



PowerShell exposes these variables in two different ways. (Actually it's the same way, but don't tell anyone..)



### So, Spit It Out Already..




First, I can access environmental variables through the provider.



dir env:\</pre>

    
    All my standard commands in navigating and retrieving items from a provider (Get-Item, Set-Item, New-Item, etc..) work in interacting with these variables.
    

    
    ### And The Other One?
    
    

    
    The other access method also uses the provider, but it accesses it in a bit different way..&#160; By using the &quot;$&quot; and the PSDrive name.
    
<pre language="powershell">$env:username



NOTE: This syntax can be used with other providers as well (eg $c:Windows)

