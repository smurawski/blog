---
layout: post
title: "Need to Test if WinRM is Listening?"
date: 2015-06-16 22:05
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


One of the most common problems I come across at Chef is diagnosing whether or not WinRM is actually listening or accessible on a remote node.  This can be a problem with 




`knife bootstrap windows winrm`




or




`knife winrm`




or in Test-Kitchen or Chef Provisioning.




### Testing from Windows





From Windows, there is a PowerShell command - Test-WSMan.




#### Checks that the WinRM service is listening





*   Replace 192.168.1.82 with the IP or hostname of the target node



`test-wsman 192.168.1.82`




#### Checks that the local account can log in via WinRM using Basic Auth





*   Replace 192.168.1.82 with the IP or hostname of the target node
*   Replace user with the local or domain user account to authenticate



`$Credential = Get-Credential user`




`test-wsman 192.168.1.82 -Authentication Basic -Credential $Credential`




#### Checks that the local account can log in via WinRM using Negotiate Auth (either NTLM or Kerberos)





*   Replace 192.168.1.82 with the IP or hostname of the target node
*   Replace user with the local or domain user account to authenticate with



`$Credential = Get-Credential`




`test-wsman 192.168.1.82 -Authentication Negotiate -Credential $Credential`




### Testing from Linux or Mac OSX





There is no Test-WSMan on these platforms, so we've got to improvise.




#### Checks that the WinRM service is listening





*   Replace 192.168.1.82 with the IP or hostname of the target node



```
curl --header "Content-Type: application/soap+xml;charset=UTF-8" --header "WSMANIDENTIFY: unauthenticated" http://192.168.1.82:5985/wsman --data '&lt;s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope" xmlns:wsmid="http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd"&gt;&lt;s:Header/&gt;&lt;s:Body&gt;&lt;wsmid:Identify/&gt;&lt;/s:Body&gt;&lt;/s:Envelope&gt;'
```




#### Checks that the local account can log in via WinRM using Basic Auth





*   Replace user:password with the appropriate local user account and password
*   Replace 192.168.1.82 with the IP or hostname of the target node 



```
curl --header "Content-Type: application/soap+xml;charset=UTF-8" http://192.168.1.82:5985/wsman --basic  -u user:password --data '&lt;s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope" xmlns:wsmid="http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd"&gt;&lt;s:Header/&gt;&lt;s:Body&gt;&lt;wsmid:Identify/&gt;&lt;/s:Body&gt;&lt;/s:Envelope&gt;'
```

