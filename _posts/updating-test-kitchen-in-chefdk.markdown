---
layout: post
title: "Updating Test-Kitchen in ChefDK"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

## Hey, New Shiny

Sometimes, you just don't want to wait for the latest/greatest features AND you want to have the convenience that ChefDK provides (without having to figure out [bundler]()).

For example, while I'm happily writing code with the current (as of this post) ChefDK 0.11.2, a newer version of Test-Kitchen has shipped which has a bunch of bugfixes and now supports cross-platform negotiate authentication.

If we weren't talking about ChefDK and just had a more vanilla Ruby environment, we could just `gem install test-kitchen` and be done with it.

ChefDK, however, use a pattern that can help accelerate the applications and ensure that they are called with the expected dependencies - using a tool called [appbundler](https://github.com/chef/appbundler).

### Enter - Appbundle-Updater

The appbundled bits in ChefDK make it a bit trickier to update versions, so there is a companion tool - [appbundle-updater](https://github.com/chef/appbundle-updater).

So, to update test-kitchen, from my PowerShell prompt:

(If I'm using `chef shell-init`)

```ruby
gem install appbundle-updater
appbundle-updater chefdk test-kitchen v1.6.0
```

(If I'm not)

```ruby
chef gem install appbundle-updater
chef exec appbundle-updater chefdk test-kitchen v1.6.0
```

That'll get you using the latest test-kitchen bits (again, versions are as of the publish date).

Happy testing!