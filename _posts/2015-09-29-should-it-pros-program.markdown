---
layout: post
title: "Should IT Pro's Program?"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

This post is inspired by an email exchange on the PowerShell MVP mailing list.

## The Eternal Debate

In Microsoft heavy shops (where I've mostly worked), there seems to be this notion that IT Pro's (what sysadmins in the Microsoft space are called) shouldn't have to dirty their hands with learning a programming language.

### The Shell Should Suffice

While PowerShell is a vast ecosystem and is growing at a rapid pace, it is not the ulitmate one-stop-shop for all your automation needs. PowerShell provides access into the .NET framework, COM, and WMI - all with the ease of running netsh.  Using those tools though, require you to learn a bit about how they operate.  Dealing with COM objects is different than working with WMI.  Heck, working with WMI via Get-WMIObject and the type accelerators is different from working with the CIM cmdlets. 

After a while, we get used to working with certain cmdlets and a handful of .NET and COM objects.  After we hit a new use case and need to use a .NET API that requires async handlers for example, the questions begin...

#### Can I do it through Powershell?

And often the answer is "yes, but..".  That trailing "but" is often the killer.  If you only know PowerShell, that can lead you down the path to a [herd of yaks desperately in need of a shave](http://www.hanselman.com/blog/YakShavingDefinedIllGetThatDoneAsSoonAsIShaveThisYak.aspx).  This is where you find out if you are truly a "Professional".

That title of IT Pro(fessional) has been the justification of many raises, promotions (had to get off that helpdesk somehow), and new jobs.  Now we need to live up to that name.

If a professional plumber came to your house to fix some plumbing and only brought an adjustable wrench and no other tools, you'd probably have a few thoughts about his/her competence.  Part of being a professional is knowing what tools you have available and picking the one that solves the problem best.  

#### So I need to learn C#?

Given that we are talking about PowerShell, many of our challenges get tied into the .NET space where the primary language used is C#.  So the perennial question I hear is "Do I need to learn C#?".

If you work supporting systems in a Microsoft shop, my answer is "*YES*".  Regardless of if you end up using C# as part of your day to day automation work, you'll have a better understanding of what each tool offers.

It will give you a greater capability to learn about new .NET libraries, which may only have examples in C#, and help you dive into the internals of problem .NET classes (via tools like ILSpy).

#### What about C++, Ruby, T-SQL, Python, or Go?

I've made the argument that C# can be a help to the IT Pro.  Other programming languages can be helpful as well.  Each language (and the header is by no means exhaustive) behaves differently and optimizes for different scenarios.  I think you should learn several languages (and each one you learn makes the next easier).  It'll expose you to different methods of problem solving and expand the tools in your toolbox.  

Example:
I have found Chef to be a great tool for extending the capabilties of DSC.  I had to learn a bit of Ruby to make that happen.  What I learned about Ruby and Chef made me better with PowerShell DSC.

#### When Do I Need To Learn Another Language?

You should start now.  The sooner you start the sooner that knowledge can make you more productive or set you up for that next role.  PowerShell is an awesome tool and has done wonders for my career.  But it hasn't been pure PowerShell knowledge that has helped me succeed.

You don't have to become an expert in multiple languages, but you should understanding their basics and being able to write some simple programs.

#### When Should I Use C# (or some other language)? 

There is no hard and fast rule here.  This is where your experience with several languages will come into play.  You can make an educated guess and pick one.  If you really don't know were to start, pick the one you are strongest in.  But be willing to recognize when another tool may be better for the job.

These days, I write a bunch of Ruby, except where it makes more sense to write PowerShell.  I know there is a trade-off in which technology I pick.  Having worked with both, I can make an educated choice about where to use Ruby and where to use PowerShell.
