---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-08-01T00:00:00Z
tags: []
title: DSC...  What is it for, really?

---

An email recently came across one of the mailing lists I subscribe to. &nbsp;I wanted to share my answer here, because this is an important discussion.


## Is DSC for Setup or Management?

<h3 dir="ltr">Yes (notice, it's not an XOR)



In my opinion, DSC (and Chef) is for setup and the management of the structural configuration of the system. &nbsp; To directly address some of the questioners initial points:


>Do I need to develop custom DSC resources (for example to create DFS folders and links) and apply a new configuration each time I have to create/or modify a DFS link? Or should I stick to the traditional system administration (I mean using PowerShell cmdlets or the GUI)?



Well, currently you'd have to implement a custom resource for DFS shares, but you don't need a custom resource per share. &nbsp;You would need to modify your configuration to add a new share, but modifying a configuration should be almost a non-event.


### These Aren't Your Grandma's Shell Scripts



If you buy into the Configuration-As-Code or Infrastructure-As-Code concept, one of the first things you want to tackle is building a pipeline to deliver those configurations to your servers. &nbsp;Whether you use a pull server setup (my preference) or you push configurations out to individual nodes, having a solid delivery mechanism makes changing configurations not much more difficult than editing a file and checking into source control (which is way easier than the "traditional" approach). &nbsp;Setting up this pipeline IS work on the front end, but pays dividends in your ongoing maintenance of your environment.


>I realize that it could be tempting to use DSC as a management system, but I think it is not a good use of DSC *at all* (please correct me if I'm wrong). Indeed, in order to manage a computer you have to modify (and reapply) the current config of the machine and that may take *more* time than using the usual cmdlets to get the work done.



If you are spending more time modifying the configuration (once you have resources and pipeline in place) than it would take to configure it on a server or series of servers, we need to sit down and have a chat.&nbsp;


### You are going to put in time



Yes, building a resource takes time. &nbsp;


Setting up a test environment takes time. &nbsp;


Structuring your configuration data takes time. &nbsp;


Building a deployment pipeline takes time.


It seems like a lot of time, when you could just RDP over the box.. or even quicker, use PowerShell remoting or some CIM cmdlets right at the box. But so does interactively managing each of your servers. &nbsp;


Over time, interactively managing (and rebuilding, and testing when the next OS ships) tends to take way more time than if you invest in your configuration testing and deployment process.&nbsp;


That's one of the advantages to the Chef ecosystem.. there's a great testing platform, which is being rev'd to make Windows testing easier (and supports DSC via the Chef DSC integration, and hopefully as a native provisioner in the near future).


### But DSC will solve my DR problem though, right?



>But if I stick to the good old traditional system admin what if my server crashes? Well, I'll need to restore my machine as usual. That being said (please react to this), in a DSC world - where I would have managed my server by reapplying a new config after each creation of DFS link (for example) - in case of crash I would only have to apply the configuration to a (bare) new machine right? This way, in a few minutes I get a fresh up and running machine with the DFS role installed AND all the configuration (DFS links recreated, etc.). Am I right???



Exactly. &nbsp;One of the benefits of using a declarative configuration management system is that you can more easily recreate the infrastructure that hosts a particular service. &nbsp;


### But it is more than DR



This isn't just a DR thing. &nbsp;Now that Microsoft is picking up the cadence of OS releases, that means the support cycle is tightening. &nbsp;


Remember the whole N-2 support for management tooling? &nbsp;When releases were 3 years apart, that gave you 9 years to stay current if you installed the latest release. &nbsp;When the release cycle is more 12 to 18 months, then you are down to 2 to 4.5 years for backward compatibility in management tooling. &nbsp;That means, to stay current, we have to be able to cycle to new server platforms faster. &nbsp;


The patching story is changing as well. &nbsp;Now there are servicing points you have to hit to continue to get patches (that's the whole story behind the "with Update" thing and why the Server 2012 R2 image on MSDN was updated to "with Update"). &nbsp;That was roughly 6 months after the OS went Generally Available? &nbsp;How quickly can you get your environment up in a test lab to test those updates? &nbsp;With declarative configuration management, just supply some different environmental data, and you can stand up near clones of your production environment for testing.


### Remember when I said SysAdmins could pull some good ideas from Development?



We need tools to help shorten the cycle in which we can deploy new operating systems and keep things patched. &nbsp;These tools exist - DSC, Chef, Test-Kitchen, ServerSpec, Vagrant, GuardRail, and more. &nbsp;These tools give us the ability to rapidly develop new resources and to build an automated pipeline that can deliver tested, high quality configurations to our servers.


We can leverage the concepts in Continuous Delivery to improve our ability to deal with infrastructure changes. &nbsp;


*   Automate all the tests we can. &nbsp;
*   Assume people checking in changes are professionals and optimize the pipeline for success. &nbsp;
*   Don't leave the pipeline in a broken state.
*   Don't be afraid to "Roll Forward" and fix things that break (vs. looking to roll back).

What would you say if you could have a process that, once patches came out on Patch Tuesday, that a process kicked off spinning up a test version of your environment, applied a patch at a time, ran smoke tests, and at the end you got a report of any breakages, and which patch things broke on? &nbsp;That'd be awesome and is totally achievable. &nbsp;
<h3 dir="ltr">Don't get laughed at

<blockquote dir="ltr">When human beings do things that the computers are perfectly capable of... Late at night the computers get together and laugh at us - [Neal Ford](https://twitter.com/neal4d)



We need to move away from manually implementing changes and manually checking changes and put automaton in place that does these standard, repeatable tasks and save the humans for the interesting, challenging work that we are better at.


 
<p dir="ltr"> 

