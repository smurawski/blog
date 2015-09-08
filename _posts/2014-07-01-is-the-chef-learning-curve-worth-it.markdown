---
layout: post
title: "Is the Chef Learning Curve Worth It?"
date: 2014-07-14 11:53
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


Wow.. I haven't even had my first day at Chef and I've landed a doozy of a question from Twitter.

 
   <blockquote class="twitter-tweet">

[@StevenMurawski](https://twitter.com/StevenMurawski) why would you want to use chef with windows instead of just DSC and Powershell? What would be main benefit / gain? Thanks
— IT Engineer Jobs (@ITEngineerJobs) [July 13, 2014](https://twitter.com/ITEngineerJobs/statuses/488286911598194688)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@ITEngineerJobs](https://twitter.com/ITEngineerJobs) DSC is a feature, not a full product.  Chef has a more complete story and is working on leveraging DSC (best of both worlds)
— Steven Murawski (@StevenMurawski) [July 13, 2014](https://twitter.com/StevenMurawski/statuses/488298084657405953)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@ITEngineerJobs](https://twitter.com/ITEngineerJobs) So it's not an either/or story.  I love what DSC is doing and how Chef is embracing that.
— Steven Murawski (@StevenMurawski) [July 13, 2014](https://twitter.com/StevenMurawski/statuses/488298334101065729)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@StevenMurawski](https://twitter.com/StevenMurawski) Okay thanks so we are saying Chef is delivering a richer experience. Is the learning curve worth it for just windows estate?
— IT Engineer Jobs (@ITEngineerJobs) [July 13, 2014](https://twitter.com/ITEngineerJobs/statuses/488366735364747266)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


## First Of All, Don't They Do The Same Thing?



DSC, first and foremost is a feature of Windows Management Framework 4. &nbsp;It consists of the


*   Local Configuration Manager (LCM), the agent that does the work
*   a DSL for generating configuration documents via PowerShell
*   a REST based pull server and compliance endpoint.

(You may be asking yourself, what about the file share pull server.. that's one of the supported features of the download manager part of the LCM.) &nbsp;


The Chef environment consists of&nbsp;


*   a server (complete with web UI)
*   a client to resolve and apply configurations
*   a workstation for developing cookbooks and managing your environment
*   testing frameworks for cookbook development
*   and more...

The part of DSC that most closely aligns to Chef is the chef client to the LCM.&nbsp;


it is possible to create much of the missing infrastructure for DSC. &nbsp;I did this at Stack Exchange. &nbsp;It took me a year and it's still very rudimentary.


So, the short answer is no, they don't do the same thing.


## But Isn't Chef Focused on Linux?



Linux is where Chef has its roots. &nbsp;Chef Server only runs on Linux and most of the community resources available appear to be for Linux or workloads you'd traditionally run on Linux (like the LAMP stack).


That does not mean that Chef isn't good for working with Windows. &nbsp;Especially since Chef is embracing DSC resources and working on incorporating them as native Chef resources. &nbsp;This means that Chef is poised to leverage all the DSC tooling that Microsoft and those in the Microsoft ecosystem develop. &nbsp;This is HUGE. &nbsp;While the DSC resource ecosystem is small at the moment, there is momentum from the PowerShell team, there are community efforts (like the PowerShell.org Community Repository on GitHub), and Visual Studio is integrating support for creating DSC configurations. &nbsp;If that's not enough, Jeffrey Snover (the father of PowerShell and now Chief Architect for Windows Server and System Center Data Center) is beating the drum at every major Microsoft conference (like Build and TechEd), as well as in interviews for people to go out and build resources.


## But, But... What About Ruby?



So you are a Windows admin and a PowerShell ninja, why should you learn some Ruby to define your configurations? &nbsp;Because, to define a configuration, you need to know VERY LITTLE Ruby ([check out this page on Just Enough Ruby for Chef](http://docs.opscode.com/just_enough_ruby_for_chef.html)). &nbsp;That jumpstart should take you all of 30 minutes to understand and with a little practice become second nature. &nbsp;


As I settle in to my new role, I'll be really looking at this area and trying to find ways to make this even easier (if possible) for Windows admins.


## Now What?



If you've stuck with me this far, I'm going to assume that configuration management is important to you (and it should be). &nbsp;Regardless of what system you use for configuration management, you are going to have some hurdles to cross, concepts to learn, and configuration syntax. &nbsp;There will be trade-offs. &nbsp;Working with DSC is mostly PowerShell (with a few new behaviors), but the ecosystem is still building and tooling is lacking. &nbsp;Working with Chef or Puppet (or Ansible or CFEngine or ...) will require a bit of Linux in your environment (guess, what - it's probably there already, somewhere) and learning a different syntax to describing your system state. &nbsp;System Center doesn't have a DSC story yet, and therefore has a few challenges for being an agile configuration management platform.&nbsp;


You should try these tools and see what is going to work for you workflow. &nbsp;In any case, you'll want to study up on concepts like version control and testing. &nbsp;You'll want to learn a bit about the concepts of refactoring and object-oriented design. &nbsp;You don't have to go deep on these topics (at the beginning), but you should get familiar, because this space isn't called "Infrastructure as Code" for nothing. &nbsp;As you begin to experiment, you'll find that learning a bit of new syntax isn't a bad thing. &nbsp;:)


## How Do I Get Started?



Take yourself over to[ learn.getchef.com ](http://learn.getchef.com/additional-resources/)and watch the Fundamentals webinar series (available on YouTube and all linked up). &nbsp;We are working on getting our Windows focused getting started material onto the site, but the Fundamentals videos will really start giving you a feel for what working with Chef looks like.


## What if I've Already Started Learning DSC?

<p dir="ltr">Keep it up! &nbsp;Learning the concepts and how DSC resources apply is going to be very beneficial, regardless of what other tooling you might end up using. &nbsp;Finding the limitations and advantages of DSC will help you evaluate any further projects, so dig deep (I'm going to keep doing so!).

