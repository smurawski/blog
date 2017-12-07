---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-09-01T00:00:00Z
tags: []
title: Why Chef? (Revisited)

---

## Setting the table



First off, let me get on record - I LOVE DSC. &nbsp;When I was at Stack Exchange, I rolled Desired State Configuration out during the Server 2012 R2 preview timeframe. &nbsp;I coddled it through the move to RTM and then to General Availability. &nbsp;At the PowerShell Summit in April, Jeffrey Snover asked how I liked the WMF5 preview. &nbsp;I had to tell him I hadn't really played much with it. &nbsp;He advised me that it had a bunch of updates for problems I reported. &nbsp;That night, I rolled WMF5 into production on my build servers.


I'm not the only one who has run DSC in a production capacity, but I was one of the first. &nbsp;While the ecosystem is building, there are still a lot of rough edges and gaps when looking to use DSC as the primary configuration management platform. &nbsp;


The more time I spend with Chef, the more I'm convinced that we have THE best platform to embrace Desired State Configuration.


## Mise en place



Let's look at what we have to work with.
<h3 class="text-align-center">DSC



*   Resources
*   Configuration DSL
*   Pattern to Separate Environmental Data
*   Pull Server
*   Linting Tool
*   Reporting Endpoint<h3 class="text-align-center">Chef



*   Resources
*   Configuration DSL
*   Pattern to Separate Environmental Data
*   Pull Server
*   Linting Tool
*   Reporting Endpoint
*   Reporting Dashboard
*   Inventory Tool
*   Analytics Framework
*   Unit Test Framework
*   Integration Test Framework
*   Cloud Integration
*   Virtualization Integration

## Inspecting our ingredients



### Resources



Both DSC and Chef offer default primitives for configuration management, as well as a way to create custom resources. &nbsp;Chef also has a preview of our ability to integrate DSC resources as Chef resources.


DSC's resources are written in Powershell or are WMI based. &nbsp;Chef's resources are written in Ruby.


### Configuration DSL



DSC provides language extensions (new keywords and an updated parsing mechanism) to PowerShell. &nbsp;The full language is available to you to aid in generating the configuration document. &nbsp;Configurations are identified by the "configuration" keyword. &nbsp;The configuration command outputs a MOF document that describes the structure of the target node.


Chef's configurations are built in Ruby. &nbsp;Like how DSC allows you to use the full power of PowerShell in a configuration command, you have the full power of Ruby when you author a recipe in Chef. &nbsp;Recipes are evaluated on the nodes they apply to. &nbsp;The chef-client processes all the recipes to run on a client and builds a resource collection to apply to the node. &nbsp;([The Chef docs have much more detail on this process.](https://docs.getchef.com/essentials_nodes_chef_run.html))


### Pull Server



DSC ships with two basic pull server options. &nbsp;The first is to use a file share. &nbsp;You stash your configurations and your custom resources out on a file share and can point your nodes to that share to look for their configurations and fun new bits. &nbsp;The second is a REST-based (actually OData based) endpoint. &nbsp;There is no UI or management tooling for this endpoint. &nbsp;It is very scalable, since all it really does is serve up files.


Chef Server (there are three flavors - open source, hosted, and enterprise), provides a central location for deploying recipes and cookbooks (how recipes and resources are packaged), as well as central data store that is searchable by nodes during their configuration application. &nbsp;There is a web UI for Chef Server that lets you explore the uploaded cookbooks, metadata, and more.


### Linting Tool



The DSC ecosystem has a contribution from [DSC Resource Kit](http://gallery.technet.microsoft.com/DSC-Resource-Kit-All-c449312d) of the xDSCResourceDesigner, which has a function Test-xDscResource. &nbsp;This command validates the structure of the resource, including validating that the schema.mof document (required metadata) matches the parameters. It recommends proper annotation of command output.


The Chef ecosystem has FoodCritic, FoodCritic has a series of built in rules and is extensible. &nbsp;FoodCritic checks everything from looking for updated metadata, to improper looping, to missing files and much more (45 rules out of the box).


### Reporting Endpoint

<p dir="ltr">If you are using the DSC REST-based pull server, you can also set up a compliance endpoint, where servers can check it whether or not they are compliant or not. &nbsp;There is no UI, just a database sitting behind it.
<p dir="ltr">On the Chef side, nodes report back to their Chef Server with the status of their inventory before the run, then their inventory and status after the run. &nbsp;Errors are viewable on the Chef Server.


### Reporting Dashboard

<p dir="ltr">If you are using DSC, you'll have to roll your own.
<p dir="ltr">The Chef Server offers a web UI for viewing the status of your organization, uploaded cookbooks, configured roles and environments, and to explore the state of nodes.


### Inventory Tool

<p dir="ltr">If you are using DSC, you'll have to roll your own.
<p dir="ltr">Chef-client includes [Ohai](https://docs.getchef.com/ohai.html), a tool that enumerates the state of the system and logs information about it. &nbsp;That information is available at the time a configuration is being processed, as well as for ad-hoc administrative efforts.


### Analytics Framework

<p dir="ltr">If you are using DSC, you'll be rolling your own here too!
<p dir="ltr">[Analytics are a new feature for Chef (Enterprise).](http://www.getchef.com/blog/2014/07/15/meet-the-chef-analytics-platform/) &nbsp;This platform allows you to do cool stuff like see changes in real-time, integrate with external systems, and tie in compliance controls.


### Unit Test Framework

<p dir="ltr">If you are using DSC, you'll be rolling your own here as well. &nbsp;We do have Pester as a good general purpose testing framework, but there is no underlying understanding of DSC.
<p dir="ltr">On the Chef side of things, we have [ChefSpec](https://docs.getchef.com/chefspec.html). &nbsp;ChefSpec actually runs a local version of the Chef Server and simulates a convergence (bringing the node back into compliance) and makes it easy to test the expected behaviors.


### Integration Test Framework

<p dir="ltr">Again, on the DSC side, we can put together our own testing framework with Pester.
<p dir="ltr">With Chef, we have [Test-Kitchen](http://kitchen.ci). &nbsp;Test-Kitchen is a test harness to help you run your recipes against a variety of types of nodes and run suites of tests against them, using a variety of frameworks (like [Serverspec](http://serverspec.org/)). &nbsp;Currrently Test-Kitchen doesn't have support for Windows guests, but that's changing. &nbsp;My co-worker, S[alim Afiune has done a lot of work to improve the Windows support](https://github.com/afiune/test-kitchen) and [Matt Wrock has a great post on getting it all to work on Windows](http://www.mattwrock.com/blog/peering-into-the-future-of-windows-automation-testing-with-chef-vagrant-and-test-kitchen-look-mom-no-ssh).


### Cloud Integration

<p dir="ltr">DSC does have some "x" resources for Azure, but no other support at the moment.
<p dir="ltr">Chef's [knife](https://docs.getchef.com/knife.html) tool has extensions for bootstrapping nodes in Azure, EC2, Digital Ocean, Google, &nbsp;and RackSpace among others. &nbsp;


### Virtualization Integration

<p dir="ltr">DSC has some basic support for Hyper-V (via the "x" resources).
<p dir="ltr">Chef has support for ESX and OpenStack. &nbsp;I'm sad to say there isn't Hyper-V support. ;(


## Putting it together

<p dir="ltr">So, when you look at the overall ecosystems, DSC makes a strong showing on it's own, but Chef rounds out the edges and provides the tooling that makes extended use and development not just possible, but fun. &nbsp;When you take into account the fact that Chef can use DSC resources, I think it's hard to debate the enhancements Chef can offer your configuration management story (and it's just going to keep getting better!).
<h2 dir="ltr">So, DSC or Chef?

<p dir="ltr">I say YES!

