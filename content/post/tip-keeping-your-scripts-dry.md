---
author: steven.murawski@gmail.com
categories:
- General
- PowerShell
- Tip
comments: true
date: 2009-06-01T00:00:00Z
tags: []
title: 'Tip: Keeping Your Scripts DRY.'

---

DRY?&#160; Does this mean I can’t use PowerShell on a water-cooled PC?



DRY is a principle that can be very familiar to the PowerShell aficionado with a development background.&#160; DRY means **Don’t Repeat Yourself**.&#160; Keeping your scripts DRY means that our scripts don’t contain repeated code.&#160; Copy/Paste is not your friend!



Why should PowerShell scripters care about keeping their PowerShell DRY?&#160; One major reason – script maintainability.



PowerShell has a huge advantage over scripting environments/shells in that the noun/verb structure lends itself to very readable scripts.&#160; If there is duplication in your code, that readability can give a PowerShellers a bit of overconfidence when reading/modifying scripts that are new to them or that have not been looked at in a while.



If you find yourself copying and pasting lines of PowerShell between different sections of your script, you could be setting yourself up for some interesting times when you need to make a change.

