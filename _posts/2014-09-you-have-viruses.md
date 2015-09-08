---
layout: post
title: "You Have Viruses!"
date: 2014-09-02 22:58
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


I had a great experience today. &nbsp;I received a phone call from someone purporting to be from Microsoft Support, who was calling because my machine was infected with viruses. &nbsp;


When he started down that line, I knew where this was going, so I spun up a virtual machine with Vagrant.


Here's a rough transcription (from memory and cutting out gems like, "look for the Windows key, it looks like four little flags")


**"Support" guy: &nbsp;I'm calling from Microsoft support. &nbsp;Your computer is infected with viruses and has been alerting us.**


*Me: &nbsp;Oh no! &nbsp;How can I we check that?*


**"Support" guy: &nbsp;I'll walk you through checking for these error messages.&nbsp;**


**"Support" guy: &nbsp;Press the Windows key and the "R" key at the same time. &nbsp;What do you see?**


*Me: &nbsp;I see a little box pop up in the lower left hand corner*


**"Support" guy: &nbsp;In the Windows box, type "eventvwr" and press return (which launches the Event Viewer)**


*Me: (I type eventvwr into the run window.)*


**"Support" guy: &nbsp;Can you read back what you typed in?**


*Me: &nbsp;"e" "v" "e" "n" "t" "v" "w" "r"*


**"Support" guy: &nbsp;OK, press enter.**


**"Support" guy: &nbsp;On the main page of the Event Viewer, look at the summaries of the types of event log entries.**


**"Support" guy: &nbsp;In the Event Viewer, expand Windows Logs and click on Application.**


**"Support" guy: &nbsp;Scroll until you see an error. &nbsp;**


**"Support" guy: &nbsp;These&nbsp;error messages were warnings from Microsoft about viruses on your computer.**


*Me: On no! &nbsp;I didn't know that..*


**"Support" guy: &nbsp;Next, on the right hand side of the Event Viewer, there is an option to "Filter Current Log", click on that. &nbsp;What do you see?**


*Me: &nbsp;(I describe the error log filtering box)*


**"Support" guy: &nbsp;Select "Errors", "Warnings", and "Critical" and click OK.**


**"Support" guy: &nbsp;What do you see? &nbsp;Those are all attempts from us to warn you about viruses on your system. &nbsp;These are why I'm calling you right now.**


*Me: &nbsp;Oh wow.&nbsp;*


**"Support" guy: &nbsp;Right click on one of the errors. &nbsp;Do you see a "Delete" option? &nbsp;No? &nbsp;That's because the error is so critical, you can't delete it.**


**"Support" guy: &nbsp;I'm going to connect you to our&nbsp;"certified" Windows engineer. &nbsp;We'll remotely connect to your machine and help you fix the problem.**


*Me: &nbsp;Thank you!*


**"Support" guy: &nbsp;Press Windows + R again. &nbsp;Delete the "eventvwr" and now type "www.infosys.net" and press Enter**


*Me: &nbsp;Ok, I did. &nbsp;*


**"Support" guy: &nbsp;Do you see a Home link? &nbsp;And the fourth item is "Suporte"? &nbsp;Click on that.**


*Me: &nbsp;Ok, I'm on that page.*


**"Support" guy: &nbsp;Click on the download button for TeamViewer.**


*Me: &nbsp;Ok, I'm downloading it. &nbsp;It's done.*


**"Support" guy: &nbsp;Run TeamViewer and give me the code and password.**


**"Support" guy: &nbsp;Now I'm going to transfer you to our "certified" Windows engineer.**


**"Engineer" guy: &nbsp;Hi, I'm connected to your computer and going to try to help you.**


**"Engineer" guy: &nbsp;So, you see all the errors in the error log? &nbsp;That was us trying to warn you about your viruses. &nbsp;Why didn't you respond?**


*Me: &nbsp;I didn't know about them. &nbsp;I've never seen that before*


**"Engineer" guy: &nbsp;What anti-virus are you using? &nbsp;**


*Me: &nbsp;Just the built in Microsoft stuff.*


**"Engineer" guy: &nbsp;(He then opened a command prompt and ran the "tree" command which printed out a long scrolling list of files and directories)**


**"Engineer" guy: &nbsp;(After that ran for a bit, he broke the execution and quick pasted a line of text that said "system error... &nbsp;system warranty expired")**


**"Engineer" guy: &nbsp;What message did the command output?&nbsp;**


*Me: &nbsp;It says something about the system warranty being expired or missing.*


**"Engineer" guy: &nbsp;Why haven't you paid for your system warranty? **


*Me: &nbsp;(At this point, I acted indignant) &nbsp;I did pay. &nbsp;Look it shows that I'm running a genuine copy of Windows!*


**"Engineer" guy: &nbsp;No, the system warranty is different. &nbsp;You didn't pay for that!**


*Me: &nbsp;(I launched into a rant about why buying extended warranties was stupid, just to waste more of their time. &nbsp;After about 5 minutes of that, I took a breath.)*


*Me: &nbsp;Oh, and by the way, you guys are full of crap. &nbsp;I'm a systems administrator and a damn good one and everything you've said is a scam. &nbsp;(Then, I didn't hear anything from the "engineer" again.)*


**"Support" guy: &nbsp;Why would you waste our time like that?&nbsp;**


*Me: &nbsp;Why would you waste mine with your scam?*


At that point, we went back to our own regularly scheduled days. &nbsp;


Thanks for the laugh anonymous scammer guys!

