---
layout: post
title: "Starting with Configuration Management?  Be Realisitic With Your Expectations"
date: 2015-06-12 15:35
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


There is a [great question](http://powershell.org/wp/forums/topic/starting-dsc-using-wmf-5-setting-company-expectations/) that came into the PowerShell.Org forums the other day from Marty Bowman. 


<h2 id="scenario">Scenario





>
  

Hello PowerShell community. I'm looking for thoughts and feedback on the following scenario at work as we start to dive into DSC.

  
  

My management has bought into the promise of DSC. I'm glad for that (truly). But, the major kick-off to this effort was to hire in a consultant for 3-months, during which we: 1) learn version control, 2) build a server baseline in DSC, 3) build a test framework (pester), 4) Do this using an agile 2-week sprint methodology, 5) at the end, deliver base OS build config via the new DSC configs; 3 of us are working on this. (Another group is doing Linux-with-Chef). This is our FIRST adventure with DSC.





<h3 id="thatsalotofchange">That's a lot of change





In his introduction Marty lays out four major objects for a three month project.  Not only are they adding one new technology - Desired State Configuration(DSC) - but they are also adding some form of source control, and a testing framework.  To top it off, they are changing their workflow to using two week sprints.




I feel for Marty.  That's a lot of change all at once.  There's also a really ambigious goal of building a server baseline (to be fair, they may have more defined requiremements than what was shared in the forum post, but this is all I have to work with).  Depending on whether or not there are existing resources they can use or if they need to develop new ones, you could spend anywhere from a day to a month building a resource.




*ASIDE: Yes, you can spend a month or more building a resource or set of related resources.  Depending on the platforms you support and the level of rigor you apply in testing it in various situations, you can spend a lot of time building quality resources.*


<h2 id="issues">Issues





>
  

Issues I’ve run into: I can’t excel at all this simultaneously. [Not to mention I could not simply ‘unwind’ from existing work on a dime.] I’ve never worked in ‘software design’ sprints before and I find it constraining and challenging for my learning process which is low-and-slow. Additionally, we decided strategically to utilize WMF 5, which you know is not production ready yet; I’m working on class-based custom resources which don’t support pull server yet. But our program manager is selling this as production ready at the end of our consulting engagement in about a month. By example, when I hear how Steve Murawski built server configs in early DSC for Stack Exchange out of 6 or 7 resources, 15 composite configs, plus all the testing – and it took him over a year – I think there’s a disconnect with expectations and reality at my organization. Another common approach I’ve heard is, ”pick something in your datacenter’ like what services should be running on servers, and use DSC for that, as a start. I’m working to get this feedback heard.





<h3 id="peoplelearndifferently">People learn differently





I learn best when thrown into the fire.  Having some sort of pressure drives me to figure out problems and really understand a technology.  That is not for everyone.  Marty shares that this learning process is "low and slow", which I take to mean he works a bit at something over a longer period of time.  That adds a hurdle to his ability to adopt their new workflow.


<h3 id="configurationmanagementisnotmagic">Configuration Management is Not Magic





The feeling I get from reading Marty's post is that their organization looks at DSC as some sort of magic that they can sprinkle on their servers and problems melt away.  




That is almost right.  




The problem with that belief is twofold.  It misses all the hard work that goes into building the automation capabilities and it doesn't account for a real understanding of the technology being managed.


<h4 id="example">Example:





You want to build a resource to manage NetLBFO NIC teaming in Server 2012 and 2012 R2.  You'll need to understand the teaming modes, what settings work well in conjuction, what impact they have on the NICs being used, how the virtual adapters behave, and more so that you can build your test and set logic correctly and write tests for various use cases.  Writing the PowerShell to do this stuff might take a couple of hours or a day, but understanding how these things work, especially if you are new to it, can take far longer.  Not to mention writing tests to validate it does what you think it does.


<h4 id="example">Example:





I can go grab some DSC resources or Chef cookbooks to stand up an ELK (ElasticSearch, Logstash, and Kibana) stack.  I know next to nothing about configuring or running those services, so when problems happen - I'm next to useless.  


<h3 id="classbasedresourcesproductionready">Class-based Resources - Production Ready?





Marty mentions they decided to work with WMF 5.  I applaud their eagerness to go with latest/greatest and the bug fixes in WMF 5 alone are worth it.  However, he's building class based resources, which have some limitations.  For production code now, I'd really stick with the initial implementation with a module and schema.mof file.  If you follow (Don Jones' advice)[https://www.youtube.com/watch?v=Upgj-IPM2UM&amp;index=4&amp;list=PLfeA8kIs7CochwcgX9zOWxh4IL3GoG05P] and pull most of your logic into your host module, reusing that in a class-based resource at a later date will be trivial.


<h3 id="mydsctimeline">My DSC Timeline





Marty mentions some of the comments I've made when first piloting DSC at Stack Exchange.  He's right, it took a long time to get basic functionality together.  One of the things that took the longest time and suffered the most changes was the structure of my configuration data and my build pipeline.  These things are critical to a successful deployment.




You want a build pipeline so that there is a standardized way to generate configs that anybody on the team can do it.  You don't want to be hamstrung waiting for a person to come back off vacation to be able to properly generate a changed configuration or fix a resource.  By setting up a build server, you are forced to codify the requirements for properly generating your configurations and this can be replicated locally by team members for testing purposes. 




*Warning: Never push to production from your workstation!  You want to make sure configs sent to production can be reproduced from your build server and source control.*


<h4 id="everyonetalksaboutresourcesnoonetalksaboutconfigurationdata">Everyone talks about Resources, no one talks about ConfigurationData





The other critical bit is your configuration data.  This structure will likely change as you add more things under configuration management, but figuring out the right layout and how to get that into my configuration scripts was a major pain.  That's why I created some of the first [DSC tooling modules](https://github.com/powershellorg/dsc) (which have since been updated by Dave Wyatt and other great community contributors).  This is one area that makes me wish I dug into Chef earlier.  Chef offers some nice patterns around environments and roles, with data bags for those things that don't fit elsewhere.




I spent weeks building functions to find the right values in configuration data so that I could tweak configuration data without having to adjust my configuration scripts. 


<h3 id="startsmall">Start small





The hard part about configuration is the two things I just mentioned, build pipelines and configuration data.  Writing resources can be time consuming and frustrating, but it's more familiar (at least to a sysadmin).  




Getting a build pipeline in place and being able to deliver a change to a single resource is a MAJOR accomplishment.  It might not feel that way, but it shows that you can deliver configuration changes via your configuration management tooling and not jumping on a box to make a change (via RDP or PowerShell Remoting).  Once you've got one item under control and you can change it as needed, you can start to add other elements to your configuration.  




When I started building my configuration infrastructure, I started with one element.  I don't remember now if it was pagefile or powerplan, but it was likely one of those things.  Once I had the ability to deliver configurations, it was a simple matter to add the next resource to my configuration script and deliver that change.  The key is that delivery pipeline.


<h2 id="impactfromthesechanges">Impact from these changes





>
  

I’ve always been highly self-directed and tend not to work well under arbitrary constraints (yeah I realize I have to deliver output somehow and within reason, that’s my job). The “motivation trifecta” of Autonomy, Mastery &amp; Purpose resonates with me. These 2-week sprints and daily standup meetings crush my autonomy and interrupt my mastery. My efforts work in bursts where periods of slowness are followed by bursts of stuff that comes out as it starts to make sense. The purpose gets distorted by the program manager when she sells it as production ready too soon. It is so frustrating because DSC is so important. For the rest of the local team working on Windows, it’s business as usual – click Next, Next, Next Finish. Maybe a few PowerShell scripts here or there ad-hoc.Getting the rest to come on-board at the right time may be somewhat like moving a boulder.







I feel for you Marty.  




I don't know how your sprints are being organized, but I believe the idea is that you pick items off the backlog for your sprint.  This leaves you in control of the tasks you pick up.  The sprint timeboxes you so that you've got a target to finish by (which can be constraining, but that might mean the tasks in the backlog are too big).  The standups are really a way to make sure everyone on the team is making progress (not stalled) as well as keep you up to date on what others are working on and finished.  




I'm sorry to hear that your program manager is selling things as production ready too early.  That's not cool and can lead to disillusionment from everyone involved when the configuration management infrastructure doesn't live up to expectations.  It also increases the likelyhood of "workarounds" that never get addressed in your CM tooling.


<h2 id="isitjustme">Is it just me?





>
  

I’ve also tried to assess whether it’s just me or the system in which I’m working (am I too slow, am I thinking about it too much?, Have we bit off more than we can chew to start?) I’m not a fast scripter but I do produce production-quality work.







It's not just you Marty.  Configuration management is hard (up front), but when done right can deliver fantastic results.


<h3 id="youwillpaythepiperhisdue">You will Pay The Piper His Due





No matter what direction you go - there is a cost. <br>
If you do traditional management with scripts and orchestration since that's "faster", you'll deal with possibly "snowflake" servers or long build out times for new servers.  




If you pick a good CM tool, you've got upfront learning costs - source control, build pipelines, testing, structuring your configuration data, but later change is faster and more consistent, environments can be replicated easily, and system stability is better.




If you pick a "fast" CM tool, you might get started quickly, but as the infrastructure you manage scales, you'll have to retrofit more scalable patterns which can be a dicey proposition.


<h2 id="tomarty">To Marty





Marty, 




I think that you are in a tough spot, but not insurmountable.




I'd really suggest checking out Don's video (linked above) on patterns for developing resources and use that to develop the module based resources (rather than the class based ones).  This will help in the "production ready" capacity and you can transition to class based resources when you are comfortable.




If you don't already, get a build pipeline in place so you can deliver a configuration with one resource to one machine from source control.  This will be your lifeline and even if you don't get a full server baseline by the end of the project, you should be able to get one in place shortly thereafter.  Resources can be updated/tweaked/fixed and if you have a solid pipeline to deliver those changes at will, you'll be able to move a lot faster.




I know you don't love sprint workflow, but try to embrace it.  If your standups are more than, "what I did yesterday, what I'm doing today, and what's blocking me" - push back.  Standups should be quick and informative and let you get back to work.  Let the sprints help show if your (company) expectations are too high!




Hang in there Marty!

