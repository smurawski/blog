---
layout: post
title: "Parameter Validation"
date: 2011-05-17 00:21
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames, parameters]
---


<a href="http://get-admin.com/blog/" target="_blank">Glenn Sizemore</a> wrote a great <a href="http://bit.ly/2011sgall" target="_blank">Scripting Games</a> wrap up post about some of <a href="http://blogs.technet.com/b/heyscriptingguy/archive/2011/05/15/simplify-your-powershell-script-with-parameter-validation.aspx" target="_blank">the parameter validation options that are available</a>.&#160; Go read it, I'll wait.



## The Smackdown - Ensuring You Have A Value




Very often you need to make sure that a parameter will not have a null value.&#160; There are a couple of options in making sure you have valid content.



### ValidateNotNull vs. ValidateNotNullOrEmpty




When you start to look at some of the validation parameters (in <a href="http://technet.microsoft.com/en-us/library/dd347600.aspx" target="_blank">about_Advanced_Functions</a>), the two that leap out are the ValidateNotNull and ValidateNotNullOrEmpty.&#160; 



Just looking at that names hints that there is something different about them, but it might not be immediately obvious.



ValidateNotNull will check if a parameter value is null when it is assigned.&#160; If it has a null value, the runtime will throw an error.



ValidateNotNullOrEmpty has some additional checks to help with arrays and strings.&#160; For example, if my function takes a parameter called ComputerName (like 



param(  [parameter()]
  [string[]]
  $ComputerName
)</pre>

    
    and I apply the ValidateNotNull parameter, 
    
<pre language="powershell">param(
  [parameter()]
  [string[]]
  [ValidateNotNull]
  $ComputerName
)</pre>

    
    I will only get an error if I pass $null
    
<pre language="powershell">c:\scripts\MyScript.ps1 -ComputerName $null

MyScript.ps1 : Cannot validate argument on parameter 'ComputerName'. The argument is null. Supply a non-null argument and try the command again.</pre>

    
    But, if I pass an empty array, or an empty string, the command will accept the input.
    
<pre language="powershell">c:\scripts\MyScript.ps1 -ComputerName &quot;&quot;</pre>
<pre language="powershell">c:\scripts\MyScript.ps1 -ComputerName @()</pre>

    
    To use validation to protect against those types of input, I can use ValidateNotNullOrEmpty
    
<pre language="powershell">param(
  [parameter()]
  [string[]]
  [ValidateNotNullOrEmpty]
  $ComputerName
)</pre>

    
    Then I will receive an error like this. 
    
<pre language="powershell">MyScript.ps1 : Cannot validate argument on parameter 'test'. The argument is null, empty, or an element of the argument collection contains a null value. Supply a collection that does not contain any null values and then try the command again.</pre>

    
    ### ValidateNotNull(OrEmpty) vs. Mandatory
    
    

    
    Neither ValidateNotNull nor ValidateNotNullOrEmpty force a parameter to be declared.&#160; The validations only have an impact if values (or lack thereof) are actually declared or passed in via the pipeline.&#160; If you would like to make a parameter required, you can set the Mandatory flag in the Parameter attribute.
    
<pre language="powershell">param(
  [parameter(Mandatory=$true)]
  [string[]]
  $ComputerName
)



This will make the parameter required and does protect against both empty strings and empty arrays, so you will not need to use either of those validation attributes.&#160; 



**NOTE: Other validations can still be done (like ValidateSet, ValidateRange, and ValidateScript).**

