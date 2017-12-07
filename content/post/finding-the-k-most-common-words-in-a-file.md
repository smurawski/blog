---
author: steven.murawski@gmail.com
categories:
- One Liner
- PowerShell
- Scripts
comments: true
date: 2009-02-01T00:00:00Z
tags: []
title: Finding the K Most Common Words in a File

---

<a href="http://dougfinke.com/blog/" target="_blank">Doug Finke</a> recently posted a blog post about <a href="http://dougfinke.com/blog/index.php/2009/02/07/powershell-find-the-k-most-common-words-in-a-file/" target="_blank">finding the most common words in a file</a>.



Doug put together a little 19 line PowerShell script to solve the issue, but something just called to me about how it wasn’t necessarily playing to some of the included cmdlets in PowerShell.



So, here’s my interpretation as a one liner:



get-content big.txt | foreach-object {[regex]::split($_.ToLower(), '\W+')} | where-object {$_.length -gt 0} | group-object | sort-object -property count -descending | select-object -property name -first 6



EDIT: One thing I’ve noticed is Doug’s script runs much faster.. 

