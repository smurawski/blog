---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-03-01T00:00:00Z
tags: []
title: What Are You Catching?

---

The try/catch block is a staple of error handling. &nbsp;In PowerShell, one common anti-pattern I see in try/catch blocks is the use of $error[0].


If you are not familiar with $error, it's an automatic variable that contains all the error records from the session. &nbsp;It's a stack, so the last error that occurred is in $error[0].


I've recently seen some public scripts that use $error[0] in the catch block

 
   <script src="https://gist.github.com/smurawski/3f28ddff96f8d17de1b0.js"></script>
 


So what happens if DoSomeLoggingOfContext throws or writes an error? &nbsp;$error will reflect the last error (terminating or non-terminating), so my LogError will be logging the wrong error and the terminating error I caught is lost.


A better option is to use $_ to represent the caught exception.

 
   <script src="https://gist.github.com/smurawski/43714eee1252cc4caf46.js"></script>
 


This ensures that you are referencing the caught error and not just whatever was last on the stack. &nbsp;


Happy Error Handling!

