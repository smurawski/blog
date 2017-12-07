---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2015-10-13T00:00:00Z
tags: []
title: Setting Up For Success

---

I get a lot of questions about how to get started with configuration management (whether DSC or Chef).  There are a few things that you can do to prepare yourself for diving into configuration management.  Ideally, you'd want to get familiar with this stuff first, since learning a bunch of new ideas all at once can be a bit overwhelming.  

## Want To Do Configuration Management?

Set yourself up for success and start learning in pieces.

## What Should I Learn?

I'm glad you asked.  Here's the short version of the things I think will get you ready to succeed with configuration management.  This is a starting place for you to explore what specific technologies and process will fit your environment.

### Source Control

Source control is a must for leveling up your IT operations game.  Treating the text documents (scripts or otherwise) as valuable artifacts that are worthy of keeping history is a major step up from "where was the latest version of that script I wrote?".  Using source control provides you a way to track what the current version of a file is, as well as a complete history of how that changed over time.  That can make tracking down errors much easier.

If you haven't used source control with your scripts (or config files, or other text-based documents), you need to start.  I don't care what source control you use, but if there is already some source control system in place at your employer that is a good start.  Then learn Git. :)  If you are getting into the configuration management space, GitHub hosts a ton of related projects and resources and being able to interact with GitHub via git will be handy.

Once you've gotten in the habit of using source control for your stuff, then spread that to your team.  Use of source control by your team is one of the first steps to a great experience moving into configuration management.  Since source control allows you to track what the current version (as well as points in time), you can easily communicate to your team (and they to you) what the current version to be used should (or the correct version for a point in time).

### Build Servers

The next step we want to take is to investigate build servers.  Since we've now got everything script/configuration-wise that we care about in source control, a build server (and build agents) are the next logical step.  

#### Why a build server and not some other orchestration engine?

Build servers are the logical choice, since they come with an implicit understanding of source control systems.  Other than that, build servers and orchestration engines serve many of the same purposes - they are just marketed to different audiences (and build servers tend to be cheaper).  

#### What is the build server supposed to do?

We'll use the build server to standardize any publishing process for scripts, configuration files, etc.. into production environments.  We want to standardize the process of deploying scripts, modules, configuration files, and other artifacts.  This will help us get away from the random, chaotic world of everyone just taking the latest thing they had laying around their filesystem (or could find scrounging on file shares) and puts in place a process that pulls from source control (our single source of truth).

Initially, you won't do much more than copy or stage current versions of files to the correct locations, but then we'll add some testing.

### Testing Tools

Now that we've decided that having a standard process for getting scripts and stuff into production, we can start adding some steps.  

#### Linting

The first step to making things more consistent is to use a linting tool.  Linting tools check for "correctness" which can be defined as best practice or development standards.  In the PowerShell world, that means PSScriptAnalyzer.  Tools like PSScriptAnalyzer come with some common rulesets and can be extended with some additional rules.

#### Unit tests

The second type of testing we'll explore is unit testing.  Unit testing is how we can validate the logic in our scripts.  We aren't testing the side effects (or what the script changes on a system), just that the right commands are going to be called with the right arguments, based on what we think the flow should be.  Unit testing tools use mocks or stubs (basically lightweight stand-ins for real commands or objects) to reduce the impact of commands that can change the actual system state.

In the PowerShell world, Pester is a common framework for writing unit tests.

#### Integration or acceptance tests

Integration or acceptance tests run the commands in a controlled environment and validate that it did what we expected it to do.  The controlled environment part is the important thing here.  Tools like Pester can validate the end state of a system, but we need to know where the system was started from to judge if we got the desired output.

##  What Next?

After you've got your team working with source control and getting familiar with the various kinds of testing, now you can start digging in to DSC or Chef.

Your first target will be to create a deployment pipeline that takes a configuration from source control and can deploy it to one node in a test environment.  From there, you can begin to scale out the number of systems you deploy to and the number of items that are configured.

