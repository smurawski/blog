---
author: steven.murawski@gmail.com
categories:
- Modules
- PowerShell Version 2
comments: true
date: 2011-03-01T00:00:00Z
tags: []
title: Connect to External Domain with the Active Directory Provider

---

When you import the Active Directory module (either on Server 2008 R2 or Windows 7 with the RSAT), a provider is added to your environment and connects to your current domain.



You can also use the provider to connect to other servers and other domains, as well as connect with alternate credentials (which is very useful as I’ve decided to live as a regular user and elevate to a domain admin account when needed).



The syntax looks like:



New-PSDrive –Name MyOtherAD –PSProvider ActiveDirectory –Server ‘mindofroot.com’ –credential (Get-Credential ‘MOR\steve’) –root ‘//RootDSE/’



The ‘Server’ parameter can be a domain (in which case it will connect to the first discovered DC), or a specific server (or server and port).&#160; <a href="http://blogs.msdn.com/b/adpowershell/archive/2009/03/11/the-drive-is-the-connection.aspx" target="_blank">Additional examples can be found here.</a>



The domain that you are connecting to either needs to be on a Server 2008 R2 DC or be running the AD Web Service.



One other caveat with the AD Provider.. the path names use the LDAP syntax, so to change directories into the default users container in the example above I would do:



cd ‘MyOtherAD:\CN=Users,DC=MindOfRoot,DC=com’

