---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-04-01T00:00:00Z
tags: []
title: Don't Mess With $OFS

---

([What's $OFS?](http://blogs.msdn.com/b/powershell/archive/2006/07/15/what-is-ofs.aspx))


I've spent the two and a half days chasing a bug in my DSC configuration generation. &nbsp;There was an extra ";`n" getting inserted between each element in the MOF, resulting in invalid configurations.


Turns out I &nbsp;had changed $OFS in my build script where I modify PSModulePath to point to the Tools directory that the build agent clones from source control (which is a temporary path). &nbsp;I never set $OFS back and it bit me later on in the configuration generation.


Another takeaway is that if you are relying on a default $OFS value, check it in your script/function/module, as it might have been changed elsewhere!

