---
title: "Adding Caching to Your Pipeline"
date: 2019-10-30T12:35:22-05:00
author: 'Steven Murawski'
tags: []
comments: true
draft: false
---

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/?WT.mc_id=social-blog-stmuraws) added [a caching capability a while back](https://devblogs.microsoft.com/devops/adding-caching-to-azure-pipelines/?WT.mc_id=social-blog-stmuraws) and I thought it was high time I enable it.

I was updating a project I maintain and [using the example from the docs](https://docs.microsoft.com/azure/devops/pipelines/caching/?view=azure-devops&WT.mc_id=social-blog-stmuraws#bundler), I was able to add [caching to my build] (https://github.com/test-kitchen/kitchen-pester/commit/4ed66dd50dc3e40fdf616db205479b1f86dc8376#diff-fec826feae04e51c0d94076385408bdc).

In the short term, I've noticed about 2 minutes per job saved by restoring the cache. (It is Ruby on Windows, so the impact may be a bit outsized to your language of choice.)

## How does it work?

The caching targets a particular file path and uses a key you specify.  The key can be a combination of files or strings that identify the current state of dependencies. In my build, I use the operating system, ruby version, and gemspec file.

## Things to be aware of

* Caches expire after 7 days of inactivity.  If you have a rarely built project, this may be of minimal benefit.
* If you want to break your cache, you can just tweak the `key` value (either add or change something) and it'll invalidate the old cache.

Give it a try in your build and see how it works! ([Read more here](https://docs.microsoft.com/azure/devops/pipelines/caching/?view=azure-devops&WT.mc_id=social-blog-stmuraws))

