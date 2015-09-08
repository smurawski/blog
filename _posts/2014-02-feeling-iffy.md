---
layout: post
title: "Feeling &quot;iffy&quot;?"
date: 2014-02-05 14:35
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


The "if" statement is such a fundamental construct in our programming and scripting experience.


It's not uncommon to see something like&nbsp;

 
   <script src="https://gist.github.com/smurawski/5b58ae56c14976b898ef.js"></script>
 



where the code starts to look like arrows pointing to the right. &nbsp;And this isn't the worst thing I've written!


## Stop the madness!



This is a great indicator of a problem or somewhere we can clean up our code (also known as a code smell).


We really don't want to get more than one or two levels deep in flow control (I'm including switch, while, do/while, foreach, and for loops here), otherwise it can start getting hard to track what is going on.


It'd be better to extract a function to wrap the conditional and leave an expressive name to describe what is happening.


Doesn't this read a bit better?

 
   <script src="https://gist.github.com/smurawski/9c980a11471ee1de7c4f.js"></script>
