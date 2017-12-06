---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2016-08-22T00:00:00Z
tags: []
title: Setting Up A Windows Workstation for Chef Development - The Easy Way

---

## The Old Way

[I've got a longer post about building a cookbook to set up for Chef development.](http://stevenmurawski.com/powershell/2015/06/building-your-chef-workstation-on-windows/)  This felt kludgy and hard to maintain, so I played around a little bit.

## The New Way

Now, I've got a couple of snippets that get me pretty well on my way.

First (from PowerShell), some basic ExecutionPolicy configuration.  Then install the latest ChefDK and Chocolatey.  After that, apply a basic Chef recipe to install Git, ConEmu, Visual Studio Code, 7Zip, WinMerge, and Vagrant.

```
# Setup my execution policy for both the 64 bit and 32 bit shells
set-executionpolicy remotesigned
start-job -runas32 {set-executionpolicy remotesigned} | receive-job -wait

# Install the latest stable ChefDK
invoke-restmethod 'https://omnitruck.chef.io/install.ps1' | iex
install-project chefdk -verbose

# Install Chocolatey
invoke-expression ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
choco feature enable -n allowGlobalConfirmation

# Get a basic setup recipe
invoke-restmethod 'https://gist.githubusercontent.com/smurawski/da67107b5efd00876af7bb0c8cfe8453/raw' | out-file -encoding ascii -filepath c:/basic.rb

# Use Chef Apply to setup 
chef-apply c:/basic.rb
```

After Visual Studio Code is installed, you'll want to install a couple of handy extensions:

* Chef - https://marketplace.visualstudio.com/items?itemName=Pendrica.Chef
* PowerShell - https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell

