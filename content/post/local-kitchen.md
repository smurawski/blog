---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2017-05-25T00:00:00Z
tags: []
title: 'Getting Local: Working on Community Projects With Test-Kitchen'

---

[Test-Kitchen](https://github.com/test-kitchen/test-kitchen) is a great project for testing infrastructure changes. (We think so much of it at Chef that we include it in the ChefDK!)

## The `.kitchen.yml`

Test-Kitchen is mostly driven by a configuration file (the `.kitchen.yml`) which will define all the elements that the test harness will use to create a system to test, how to communicate to that system, what configuration to apply, and how to test that configuration.  You'll find a `.kitchen.yml` in just about every Chef community cookbook.

As you might guess from the file extension, this file uses YAML markup.

### Drivers

The first element to consider in the configuration (order for top level items is not important in the config file) is the **driver**.  The driver is how Test-Kitchen will interact with whatever tool will be creating the systems to test.

Examples of tools with Test-Kitchen drivers are Azure ARM, Hyper-V, Vagrant, EC2, Docker, and VSphere. You can find all the plugins the Test-Kitchen maintainers know about in [this handy list](https://github.com/test-kitchen/test-kitchen/blob/master/ECOSYSTEM.md).

Example (of the `kitchen-hyperv` driver config)

```
driver:
  name: hyperv
  parent_vhd_folder: c:/hyperv
  memory_startup_bytes: 1073741824
  processor_count: 2
  vm_switch: LAN
  enable_guest_services: true
```

### Transport

Now that we've defined a driver to create (and later destroy) our systems to test, we need a way for Test-Kitchen to communicate, issue commands, and copy files to the system under test.  This functionality is handled by the **transport**.

Test-Kitchen has two out-of-the-box transports, SSH and WinRM.  SSH is the default transport, unless your system under test is identified as a Windows node (more on this in our discussion of platforms below).

*NOTE*: Users in heavily proxied/firewalled environments may face some challenges here, especially when dealing with systems that are spun up on various cloud services.  The workstation or server running Test-Kitchen will need to communicate out over SSH or WinRM (depending on the type of system under test).

The transport will need to be configured with a username and password (or ssh keys) if the driver does populate those settings for you (many do).

Example (of the `winrm` transport)

```
transport:
  name: winrm
  elevated: true # The WinRM transport has an "elevated" mode that runs all commands through a scheduled task to avoid some of the headaches running under WinRM.
```

### Platforms

We've now got a way to create systems under test with our driver and we can talk to them via our transport.  The next bit we need to define are the **platforms**.  The platforms are the system images we are going to test against.  Test-Kitchen will create a test matrix based on the platforms defined and the suites (coming up shortly).  This matrix provides us a list of instances (suite+platform) for Test-Kitchen to apply configurations and run tests against.

Platforms include operating system and shell details (which can be used to infer what transport to use).  For example, Test-Kitchen assumes that a platform is some variant of Linux unless the name starts with `win`.  So the default assumption would be that the transport should be SSH.  Platforms that start their name with `win` or have their `os_type` setting set to `windows` 

Example

```
platforms:
  - name: windows-server2012R2
    os_type: windows # this setting would be inferred because of the name
    shell_type: powershell # this setting would be inferred because of the os_type
  - name: windows-server2016
```

### Provisioner

Once we've got a system spun up and we can talk to it, the provisioner takes over.  The **provisioner** is how we are going to apply a configuration to the system under test.  There are a number of provisioners, covering Chef, DSC, Puppet, Ansible, and even just shell scripts (and more).  Each provisioner will have their custom settings and the best resource for those is the individual project pages for those plugins ([see the Ecosystem doc for more.](https://github.com/test-kitchen/test-kitchen/blob/master/ECOSYSTEM.md))

Example

```
provisioner:
  - name: dsc
    dsc_local_configuration_manager_version: wmf5
    dsc_local_configuration_manager:
      reboot_if_needed: true
      debug_mode: none
    configuration_script_folder: .
    configuration_script: SampleConfig.ps1
    gallery_uri: https://ci.appveyor.com/nuget/xWebAdministration
    gallery_name: xWebDevFeed
    modules_from_gallery:
    - xWebAdministration
    - name: xComputerManagement
      requiredversion: 1.4.0.0
      repository: PSGallery
```

### Suites

**Suites** are how we define for the provisioner what to apply. For every suite defined, Test-Kitchen will create an instance per platform.  We can customize settings for any of the previous configuration items.

Example

```
suites:
  - name: MSFT_xWebsite
    provisioner: 
      configuration_script_folder: Tests/Integration/MSFT_xWebsite
      configuration_script: MSFT_xWebsite.config.ps1
      configuration_name: MSFT_xWebsite_Config
  - name: MSFT_xWebsite_NewWebsite
    provisioner:
      configuration_script: Sample_xWebsite_NewWebsite.ps1
      configuration_data:
        AllNodes:
          - nodename: localhost
            websitename: test
            destinationpath: c:/sites/BakeryWebsite
            sourceurl: 'http://blogs.msdn.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-63-74-metablogapi/3124.Demo_5F00_WindowServer2012R2_2D00_Preview_5F00_4677B514.zip'
```  

### Verifier

The final component of this test framework is the **verifier**.  The verifier is how we test that the configuration done by the provisioner did what we expected it to do.  This is what will drive our level of trust in the configuration management we are going to apply to our systems.

By default, Test-Kitchen looks for tests for the verifier in `./tests/integration/{name of suite}`.  Individual verifiers may have additional requirements.

Example

```
verifier:
  name: pester
```

## Customize Without Replacing

### Getting Local

When we are working on internal projects or personal projects, we get to set the defaults.  However when we are working on a shared project (like a community cookbook or DSC resource), it may come with a `.kitchen.yml` that uses a driver and/or platforms we don't use. Or maybe it doesn't have a suite defined for the scenario that matters to us.  In these cases, we can define an override file - the `.kitchen.local.yml`.  This file takes all the same type of configuration options as defined above, but they can override or augment the settings in the `.kitchen.yml`.

### Make The Most Of the Basics - Environmental Overrides

The file name and location convention for the `.kitchen.yml` and the `.kitchen.local.yml` can be overriden by two environment variables, `KITCHEN_YAML` and `KITCHEN_LOCAL_YAML` respectively.  I use this to maintain some shared override files for my cookbook testing work.  Locally I use Hyper-V and cloud-wise I use Azure, so I have some override files that define the windows platforms I need to test against under each of those.  When I go to test a cookbook, I can use the `KITCHEN_LOCAL_YAML` environment variable to point Test-Kitchen to my overrides for driver and platforms in a consistent way.

## Troubleshooting your `.kitchen.yml`s settings

With all the possible settings and now with additional override files, how can we know what configuration options are actually getting used by Test-Kitchen?  This is where `kitchen diagnose` and `kitchen diagnose --all` come in.  One of the most frequently requested troubleshooting steps (when there are issues with an attempt to use Test-Kitchen) is for the output of `kitchen diagnose`.  This will process the configuration files and apply defaults to settings that aren't specified.  The first step is to make sure that we are seeing the right settings being applied.  The `--all` switch provides insight into what config file settings are coming from as well.
