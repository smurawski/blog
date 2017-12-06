---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2015-05-01T00:00:00Z
tags: []
title: Time For a Change (of Environmental Variables)

---

Environmental variables can change for a number of reasons and there are a few key things to remember about why you likely won't see those changes reflected immediately.




*   Processes get their environmental variables from their parent process.
*   Not everything that changes an environmental variable sends a broadcast that the settings have changed.
*   Only top-level (below Windows Explorer) processes can listen for those broadcasts.
*   Not every process reacts to those messages - PowerShell, cmd.exe, conemu, and more do not pick up changes to environmental variables.
*   Anything spawned by the processes mentioned above will get stale environmental variables.  And anything they spawn will get the same.
*   Child processes cannot update the environmental variables of their parent process (just like in real life, the parents' don't listen to the kids).



This leads to confusing situations like:




You are running cmd.exe or PowerShell inside of ConEmu, you will not see any change to environmental variables until ConEmu restarts.




You are running chef-client and you install an MSI, which updates your PATH.  chef-client will not see that updated path until the next time it runs (in a new host process - so if you are running as a service it might be a while...).

