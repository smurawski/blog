---
layout: post
title: "Cooking Up Some Changes"
date: 2014-06-30 16:00
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


Working at Stack Exchange has been one of the most rewarding experiences of my career. &nbsp;The developers and sysadmins there are top notch and we got to (and they continue to)&nbsp;work on interesting problems and deliver quality solutions. &nbsp;


However, I recently gave my notice at Stack Exchange. &nbsp;I'm entering my last two weeks on the job as an Site Reliability Engineer there. &nbsp;


### I think the time has come to move on to my next challenge.



I've been following the configuration management space pretty closely and been diving deep into the Desired State Configuration space (DSC) and I've become a big fan of Chef and how they are approaching Windows Server as a platform as well as how they embrace community.


I've become a big enough believer in that strategy that I'm going to work for Chef as a Technical Community Manager. &nbsp;I'll be starting with Chef in mid July, after I wrap up some outstanding projects at Stack Exchange.


## But isn't being a Site Reliability Engineer at Stack Exchange a Dream Job?



Yeah, it is. &nbsp;I'm so very grateful for my time and experiences with the Stack Exchange team. &nbsp;They push the boundaries of operating systems, application stacks, and hardware on a daily basis and share those experiences with everyone, through open source contributions, presentations, and various other means. &nbsp;


While the team isn't perfect, they try very hard and everyone strives to get better and make the team better overall. &nbsp;They embraced me and supported all my community endeavors (which were a few and made some projects a bit challenging with scheduling). &nbsp;Working at Stack Exchange gave me a view of how the "other half lives" in letting me live and work a DevOps lifestyle and push my role from SysAdmin towards Site Reliability Engineer. (Want to know the difference? [Check out this post from Matt Simmons](http://www.standalone-sysadmin.com/blog/2014/06/the-difference-between-site-reliability-engineering-system-administration-and-devops/).)


## Why the change?



### <span style="font-size:15px">I think.. It is THE time for Declarative Configuration Management, especially in Windows Server environments.</span>



Between the announcements and sessions at Build, the PowerShell Summit, TechEd, ChefConf, and other industry events and the level of interest I've seen, the interest in different declarative configuration management technologies is ramping up way faster than I've seen with other technologies in the Windows Server admin community and companies with significant Windows Server roles.


Chef has shown me their commitment to community and to Windows Server as a platform. &nbsp;I think, of any current configuration management platform, Chef is best poised to provide a fully integrated configuration management experience. &nbsp;They are working on better support for DSC Resources, which Microsoft is investing in the development of internally and encouraging the development of externally.


## What'll I be doing?



In my new role, I'll be responsible for helping make it easy for people to find, join, and participate in the Chef community. &nbsp;As one might guess, my main focus area will be on the sysadmin community that supports Windows-based infrastructures. &nbsp;I'll get to dive in to how Chef plays with PowerShell and DSC and share those experiences with you.


I hope you'll join me in my exploration of how Chef can make configuration management in Windows Server environments delightful. &nbsp;I'll especially want to hear where you are having trouble. &nbsp;My job is to make that a great experience, but I'll need your help to make that happen.


## So what's going to happen with my DSC efforts at Stack Exchange?



While I may have been the face of DSC at Stack Exchange, the rest of the team is sold on configuration management and pursuing DSC on the Windows platform. &nbsp;The team at Stack Exchange is going to continue making their DSC experience more awesome and I'm sure you'll hear from them about the good (and the bad!) of their experiences.


### Are you going to try to convert them to Chef?



No! &nbsp;The team at Stack Exchange has some good Puppet expertise on staff and converting the existing Puppet environment (which is working well and getting better daily thanks to mainly the work of Shane Madden and Tom Limoncelli on the repository and George Beech on the Vagrant test environments) would be a major undertaking. &nbsp;


The DSC environment would likely transition to Chef more readily, but the main reason that Stack is sticking with DSC is for the lighter weight DSC local configuration manager (and no additional runtimes).


If they ever expressed the interest to move to Chef, I'd happily help them however I could.


## And what about the PowerShell.Org Community Repository?



I'm **definitely**&nbsp;going to continue exploring and working in the DSC space. &nbsp;I'll continue to make the PowerShell.Org repository a great location for DSC resources and contributing to the DSC body of knowledge at PowerShell.Org. &nbsp;I'm also going to be diving deep into how Chef leverages DSC and PowerShell to make configuring the Windows Server platform a first rate experience.

