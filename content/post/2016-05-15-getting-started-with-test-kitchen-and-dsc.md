---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2016-05-15T00:00:00Z
tags: []
title: Getting Started With Test-Kitchen and DSC

---

## The Short Version

* [Install ChefDK](http://stevenmurawski.com/powershell/2015/06/want-the-latest-chefdk-on-windows/)
* From a new PowerShell window

`chef gem install kitchen-dsc kitchen-pester kitchen-hyperv`

* Change your directory to one with either a DSC configuration script or a PowerShell module with DSC resources.
* Put a .kitchen.yml document in your working directory ([like this example](https://gist.github.com/smurawski/2feccb3aff30153b4ba3))
* Type

`kitchen list`


### NOTE:

The .kitchen.yml example expects that you'll be using a local Hyper-V instance and that you have a base virtual machine image to work from.

It expects that you'll be in the root directory of a PowerShell module with DSC resources (class-based or traditional).

It expects that you'll have an Examples folder with a script called dsc_configurations.ps1 that will have a configuration named Default.

It expects any Pester tests you want to run will be in `./tests/integration/default/pester`.

All of those things are customizable.

### Additional documentation

[My talk for the Mississippi PowerShell User Group on DSC and Test-Kitchen](http://mspsug.com/2016/05/17/video-acceptance-testing-powershell-desired-state-configuration-with-test-kitchen/)

[Test-Kitchen docs](http://kitchen.ci)

[Test-Kitchen project](https://github.com/test-kitchen/test-kitchen)

[kitchen-dsc](https://github.com/test-kitchen/kitchen-dsc/blob/master/README.md)

[kitchen-hyperv](https://github.com/test-kitchen/kitchen-hyperv/blob/master/README.md)

[kitchen-pester](https://github.com/test-kitchen/kitchen-pester/blob/master/README.md)

[Example project - xWebAdministration](https://github.com/smurawski/xWebAdministration)
  
