---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-04-01T00:00:00Z
tags: []
title: Tracing a Specific Section of Code

---

I don't know if you've had to trace down a problem in a PowerShell script before, but sometimes it's helpful to watch how the code progresses (see what loops run, what gets skipped, etc..). &nbsp;<a href="http://go.microsoft.com/fwlink/p/?linkid=289612" data-cke-saved-href="http://go.microsoft.com/fwlink/p/?linkid=289612" target="_blank">Set-PSDebug</a>&nbsp;-Trace 1 can help you find there.


If you are dealing with large amounts of code, that output can quickly become overwhelming. &nbsp;If you want to narrow down the code you are watching you can use this little trick to conditionally enable tracing...

 
   <script src="https://gist.github.com/smurawski/10274011.js"></script>
