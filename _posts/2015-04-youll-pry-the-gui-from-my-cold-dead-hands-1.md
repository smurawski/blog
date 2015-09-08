---
layout: post
title: "You'll Pry the GUI from My Cold Dead Hands"
date: 2015-04-16 14:32
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---
<h2 id="iwontusewindowsserverifthereisnogui">I won't use Windows Server if there is no GUI





Get over yourself.  We've had remote management tools for years (going back to the admin pack during the Server 2003 days).  You need at least basic command line skills to do simple things like checking network connectivity, formating file systems, and copying files.

 
   <blockquote class="twitter-tweet">

"If you don't want to learn anything new, don't work in IT" via [@jsnover](https://twitter.com/jsnover) on [@Geek_Whisperers](https://twitter.com/Geek_Whisperers) [http://t.co/2cjxDNLBHj](http://t.co/2cjxDNLBHj)
— Steven Murawski (@StevenMurawski) [April 11, 2015](https://twitter.com/StevenMurawski/status/586948016646426624)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 
<h2 id="ifyouwerenotwillingtomakethateffort">If you were not willing to make that effort





and you worked for me, I'd fire you. Today.


<h4 id="youarenotaprofessional">You are Not a Professional





If the title and first header match statements you've made or make you are not an IT professional. <br>
If you will discard a platform solely based on that one element, despite all the potential advancements it brings that could help improve your environment for your business, then you are not a professional.




*   Professionals keep abreast of changes in their ecosystem.
*   Professionals identify beneficial changes and work to integrate them into their environment
*   Professionals identify the challenges the new platforms offer and figure out how to mitigate those.

<h2 id="butmyteamdepartmentbusinesswontmovetothat">But My Team/Department/Business Won't Move To That





Your business (unless you are in a very technical space) very likely doesn't give a piling of steaming cow feces about what server operating system you run.  They care that services are online, new capaibilities can be rolled out effeciently, and for a better cost than they can buy from "the cloud".




Regardless of what you roll out in production, you (as a professional) need to be experimenting and learning.  You need to be ready when the demand from your current role or some future one puts that challenge in front of you.


<h3 id="server2008r2isnotapanacea">Server 2008 R2 is not a panacea





If you are rockin' and rollin' with lots of Server 2003 in your environment and your plan to migrate is to 2008 R2, you are only buying yourselves 5 years max (it is out of mainstream support now and onto extended support.


<h3 id="theminshellcorenanodirectionhasnotbeensecret">The MinShell/Core/Nano Direction Has Not Been Secret





Starting with Server 2008, Microsoft has been beating the drum that stripped down server installs were the way to go.  It has been a long process but with each subsequent release, Core has been more capable.  




With Server 2012, MinShell entered the fray to help provide a glide path by keeping the local administrative tooling.




With Server 2016, we are getting the result of that direction - Nano.  Nano is a full break with the past, cleaning out legacy x86 support and tons more.




If you are not building your remote management experience, you will be in a tough place in the coming years.  Software, Platform, and Infrastructure as a Service are all coming for your job.  If you aren't ready to offer a competitive advantage to your company, I don't hold out much hope.


<h1 id="cometothelight">Come To The Light





There is a better way.  Release the fear of no GUI on the server.  It's still safe at home on your management workstation or management server (yeah you can have one of those).


<h4 id="1embracetheshell">(1) Embrace The Shell





There is so much reusable PowerShell script out there...  Learn the basics, get comfortable on the command line.  It'll hurt a bit at first, but it does get better.


<h4 id="2usesourcecontrol">(2) Use Source Control





(GASP!) What?  That's for developers!  Except things like the State of DevOps report show that one of the key indicators of a strong business are if the IT operations personnel use source control. <br>
The more scripts you write (or borrow), the more important source control becomes.  Whether you are a team of one or hundreds, source control offers a ton of benefits.


<h4 id="3learnaprogramminglanguage">(3) Learn a Programming Language





PowerShell can double as a programming language, but branch out and learn C# or Ruby or Go.  Learn T-SQL.  Expand your capabilities.  You can script some things, but others can be better served in some other language.


<h4 id="4investinconfigurationmanagement">(4) Invest in Configuration Management.





Learn Chef, DSC, Puppet, Salt, Ansible, or something else (I recommend Chef and DSC).  Start bringing your infrastructure under control.  Don't jump right to this.  Get comfortable with the three steps above first.


<h4 id="5test">(5) Test





Test your infrastructure, test your scripts, test patches, testnew software installs.  Doing this requires some automation, but now that you've been learning programming languages and using source control plus your command line experience, that shouldn't be a problem.




Once you've got a start on getting your infrastructure under control and can get some tests around its state, moving to new operating systems becomes less daunting.

