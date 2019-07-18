---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2019-07-19T00:00:00Z
tags: []
title: Migrating the Test-Kitchen Project to Azure Pipelines

---

Last year, [Azure Pipelines announced some benefits for open source projects](https://azure.microsoft.com/en-us/services/devops/pipelines/?WT.mc_id=test_kitchen_migration-blog-stmuraws) like unlimited build minutes and 10 free parallel jobs. After Microsoft Ignite | The Tour slowed down, I had a bit of time free up. I've been involved with the [Test-Kitchen](https://github.com/test-kitchen/test-kitchen) project since my time at Chef. I reached out to some of the other maintainers to see if they'd be interested in moving the project over to Azure Pipelines. They were receptive, especially if we could get the build times down. As a cross-platform project, Test-Kitchen used two CI services to run tests on Windows and Linux. The project would be successful if we could streamline the number of services being used and maintain or improve the build times.

## Creating The Build Definition

Since they were open to the idea, I began coverting the builds as they were implemented on the other services. Most of the build ported directly command for command. With a bit of (well, a lot of) referencing to [the YAML schema reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema&WT.mc_id=test_kitchen_migration-blog-stmuraws), I was able to get a mostly functional build going.

### The First Detour

The first challenge I hit was trying to make a preemptive optimization. I started to break up the Windows build steps in an effort to make those go more quickly. On the previous platform, the Windows build tested one Ruby version and builds took about 12 or 13 minutes. As I separated out the steps, I found that Azure Pipelines workers were fast enough that I could keep all the steps together, as well as run them against multiple Ruby runtimes.  The overall build times for the Windows builds came in a bit over 7 minutes.

### Next Up, Upcasing The Environment

After I was comfortable with the state of the Windows builds, I began to port over the Linux builds.  Now, the Linux builds had a bit more going on, including some integration testing with Docker.  Most of the testing moved over well, with minimal changes.  One small gotcha was that Azure DevOps makes all build variables names or environment variables names that are being passed in uppercase.  I had to tweak a few things to ensure that the right environment variables were being looked at.

I had one other environmental problem, I passed in an environment variable two high up the hierarchy and it polluted the state of some of the functional tests.  Fortunately, you can pass in [variables or enviornment variables at just about every level](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch&WT.mc_id=test_kitchen_migration-blog-stmuraws). A quick tweak fixed something I spent way too much time trying to replicate.

### Sudo? No Tools For You!
Then came the most frustrating part of this process. Part of the Test-Kitchen CI includes integration tests that require the installation of a proxy.  Test-Kitchen wants to ensure that when proxy settings are specified, all parts of Test-Kitchen respect and use them. This requires having to `sudo` or escalate privileges to install and manage services. In this process, I was forced to add a bit of cruft that is pretty fragile to the build. (I have talked to the product team and we have an issue to track the problem of this behavior.  [You can read more about it in the issue.](https://github.com/microsoft/azure-pipelines-image-generation/issues/1092).)

The short version of the problem is that while there is a task to choose a Ruby runtime and add it to your path, when you `sudo` the path is replaced with a `secure_path`.  This led to me being able to run `ruby`, but not `gem` or `bundle` or other binstubs that were in the `bin` directory of the Ruby install.

To work around this, I have a build step to create a file in `/etc/sudoers` that updates the path with the Ruby version I'm using. (I've since found a better option - exempting the group the `vsts` user is part of - which won't be as brittle.  I may change it if the base image doesn't change it first.)

I added a snippet of script like:

```
sudo echo 'Defaults	secure_path="/opt/hostedtoolcache/Ruby/2.5.5/x64/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"' | sudo tee /etc/sudoers.d/kitchen
```

It's ugly and I'm sure this will only be a point in time problem. I now had a repeatable build that covered all the same test cases on Windows and Linux.

### Let's Throw In Some Macs Too

Since [Azure DevOps has Windows, Linux and Mac hosted agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&WT.mc_id=test_kitchen_migration-blog-stmuraws), and I had the build steps down, I figured I'd add a Mac build job as well.

In all, working at it in increments, I spent [about 22 hours experimenting and tweaking the experience to ensure a smooth cutover](https://wakatime.com/@5d9628b4-4d48-4274-9697-46fc5cc0bf42/projects/xajnwoiwsw?start=2019-07-01&end=2019-07-18).  Some of the time was spent digging into future changes, like [using container images](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/container-phases?view=azure-devops&tabs=yaml&WT.mc_id=test_kitchen_migration-blog-stmuraws) to ensure current Ruby versions.

I'm very grateful to the other maintainers of the Test-Kitchen project for their willingness to explore Azure Pipelines and I think we've got a good experience going.

I'm going to continue to move some plugins that I work on for Test-Kitchen to Azure DevOps and help any other plugin maintainers move as well.
