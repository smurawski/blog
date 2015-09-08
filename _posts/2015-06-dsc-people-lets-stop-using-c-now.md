---
layout: post
title: "DSC People - Let's Stop Using 'c' Now"
date: 2015-06-08 15:43
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---
<h2 id="enoughalready">Enough Already!





The 'c' prefix for DSC Resources was proffered up by Microsoft as a way to fork their experimental ("x" prefixed) resources since they couldn't get them released in a way that they could take contributions.  This provided a way for the community to update those resources and share them back, without a name collision.




That's all changed now with the [PowerShell Team's GitHub](https://github.com/powershell) organization.  Now, we can contribute directly to the PowerShell Team's community resources.


<h2 id="justnameitnicely">Just Name It Nicely





There is nothing magical or special about resource name prefixes.  Neither "x" nor "c" imbue any greater level of confidence in quality or coverage.




Due to how DSC resources are loaded (using Import-DscResource) you need to qualify what module resources are coming from, so naming conflicts should be a minimal concern.




So let's just let this shameful piece of legacy "x" and "c" waste pass by and just name things appropriately.

