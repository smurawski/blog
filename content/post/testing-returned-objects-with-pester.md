---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-02-01T00:00:00Z
tags: []
title: Testing Mocked Output with Pester

---

I've been using [Pester](https://github.com/pester/Pester) recently for unit testing of my scripts. &nbsp;One problem I have in testing some scripts is that it's difficult to test functions that call methods of objects returned.


It's easier to explain with an example.The below script is a function from my [PageFile DSC Resource.](https://github.com/PowerShellOrg/DSC)

 
   <script src="https://gist.github.com/smurawski/599ed82c89fb092d936a.js"></script>
 


In order to test this function, I need to return an object from my Get-WmiObject call and test if the right values were set on the object and that the Put method was called. &nbsp;For this example, I'll focus on the Put call around line 23.


## Do No Harm



Strategy one would be to actually call Get-WMIObject and test the current value. &nbsp;Since this test could be run by anyone on my team (or since the script is public, by anyone elsewhere too), I don't necessarily want my test re-configuring people's systems. &nbsp;


## I Do Not Think That Does What You Think It Does



Pester provides a handy way to do that by mocking commands. &nbsp;A mock in this sense is a function that shadows the original function and let's me validate that it was called and to control what output it returns. &nbsp;There's some decent documentation around this in the Pester wiki.


## I Can't See You...



I need to extend that concept a bit further and have my mock return an object that can track what happens to it. &nbsp;Since my function will never return that object, I can't inspect it directly. &nbsp;I need to set something in the global scope so my tests can see if the correct behavior was followed.

 
   <script src="https://gist.github.com/smurawski/ea3c8f20dfe54bb1c604.js"></script>
 


## Remember Me?



In this mock, I use [New-Module](http://technet.microsoft.com/library/e4d0486e-3235-441b-8d9b-4485985efd04(v=wps.630).aspx) to return a custom object with the property I need to test and and a replacement for the method I need to call. &nbsp;This method will create or update the global variables to indicate if it was called and what the values of the object were at the time it was called.


My full test looks like this.

 
   <script src="https://gist.github.com/smurawski/6de5a7f97e495d30301a.js"></script>
