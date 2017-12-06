---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2016-01-20T00:00:00Z
tags: []
title: What Do You Expect From Windows Containers?

---

## What they are and what they aren't

I've been fielding a lot of questions about windows containers, so here's a quick brain dump of one of the common misconceptions.  One of the biggest is whether or not you can run multiple versions of Windows server in containers.

### I can run different major OS versions in containers - FALSE

Unlike Linux, where OS distributions are more decoupled from the kernel versions, each major version of Windows server is a different version of the kernel.  So, as things stand now, containers are only available on Server 2016 (currently in technical preview) and the container images are only for that OS version.  You cannot create images for Server 2008 R2 for example to run on a 2016 container host.  They are different kernel versions.
