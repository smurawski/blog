---
title: "Patching With Chef"
date: 2017-12-21T09:41:47-06:00
author: 'Steven Murawski'
tags: []
comments: true
---

One of the more common questions I hear is "How do I patch with Chef?".

## Don't!

For the most part, managing the patch state of individual states of the patches across a system is the path to madness (or at least a significant bar tab).

<div style="width:100%;height:0;padding-bottom:100%;position:relative;"><iframe src="https://giphy.com/embed/1BXa2alBjrCXC" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/1BXa2alBjrCXC">via GIPHY</a></p>

## What Should We Do?

Chef **should** be managing the upstream sources of your patches.  (You are running your own WSUS server, right?)  The upstream source management tools (for whitelisting patches, reporting, etc..) tend to be much stronger, especially in the Windows world.

Chef **should** be configuring your update schedule.

Chef **can** be used to install/ensure one-off patches for critical vulnerabilities.  These one-offs or criticial situations should eventually be part of your baseline patching and be able to drop off your configuration management list.

## Why Shouldn't We Manage Patches With Chef?

The Windows update API is painful and slow (in part because it has to call out to either Windows Update or a WSUS server).  In order to effectively manage your patches, you'd need to rebuild an in-memory graph of patches installed, things that are deprecated/superceded, and patches available.  WSUS uses a SQL Server database for this and it can be slow to respond.  Trying to do this on each Chef run will add tremendous overhead to your configuration management runs.

The move towards patch rollups may simplify this, but the API slowness and limited availability in non-interactive contexts (like PowerShell remoting) still lead me away from individual patch management.

## What Cookbooks Should I Use?

I'd start with the [wsus-client](https://supermarket.chef.io/cookbooks/wsus-client) and [wsus-server](https://supermarket.chef.io/cookbooks/wsus-server).

<div style="width:100%;height:0;padding-bottom:138%;position:relative;"><iframe src="https://giphy.com/embed/MS3XuWjQV1FiU" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/dancing-party-birthday-MS3XuWjQV1FiU">via GIPHY</a></p>

Happy Patching!