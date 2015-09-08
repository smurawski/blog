---
layout: post
title: "Keep It Short"
date: 2014-01-30 23:29
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


PowerShell is much more readable (and understandable when variables and functions are well named and the scriptblocks are kept short. &nbsp;
<p id="yui_3_10_1_1_1391784710853_26979">If I have to scroll and scroll and scroll to read one script/function, it's much harder to follow. &nbsp;
<p id="yui_3_10_1_1_1391784710853_26980">It's also easier to track down logic errors or a lack of error handling, since you are dealing with much smaller pieces of code.
<p id="yui_3_10_1_1_1391784710853_26981">For example:

 
   <script src="https://gist.github.com/smurawski/d8df458c13f7db87e4e8.js"></script>
 


<br>
<p id="yui_3_10_1_1_1391784710853_28628">vs.

 
   <script src="https://gist.github.com/smurawski/c7e72c71d60826cd7b29.js"></script>
 
<p id="yui_3_10_1_1_1391784710853_28629"><br>
<p id="yui_3_10_1_1_1391784710853_28630">**Which would you rather read?**
<p id="yui_3_10_1_1_1391784710853_28631">If you'd like to see the "full" set of functions, [take a look at this Gist](https://gist.github.com/smurawski/6de63da2c9af6f4cf595).

