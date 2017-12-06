---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2015-04-01T00:00:00Z
tags: []
title: I'm Too Busy To Test

---

## Or My Boss or Client Will Not Give Me The Time





So, they want you to create a script/function/module/whatever and you deliver a working, fully functional piece of work product from the first line you type to the end with no experimentation or testing in the middle?  




I need to work with you because I've never seen that happen, except in the most trivial of cases.  I think I'm pretty good with a line of code, and I make plenty of mistakes (check out the history of any of my projects).  




# We are all testing





The dirty little secret around testing is that we all do it, whether it is running commands in a test environment or walking through the steps in an iterative process.




Most employers/clients don't know what it takes to get that fully functional bit of script - that's up to you as the professional crafting it.  




When the employers/clients are asked, "should I write automated tests with that?" or "can I include some automated tests with that?" and the alternative is "I can just give you the working code" - of course they are going to take the latter.




Writing automated tests is part of building working code.  It is an internal function to the development process, not to be treated as an external deliverable. (This changes slightly in the open source space, where tests are commonly required.)  




Automated testing allows you to more quickly change the code you've written, maintaining functionality and improving quality.




Every time you revisit a part of your script or function and make a non-trivial change, you risk making a change to the behavior of the program.  Having an automated test harness gives you a safety net to catch unintentional changes, verify desired changes in behavior, and make sure bugs stay fixed.




You are lying to yourself if you think this isn't happening manually as part of your process.  There is some level of testing happening.




## So





The question isn't "Can I write tests?" 




The question is "What manual parts of my development process can I replace with automated tests?"

