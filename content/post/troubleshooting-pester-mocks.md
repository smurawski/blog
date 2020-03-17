---
title: "Troubleshooting Pester Mocks"
date: 2020-03-17T16:35:36-05:00
author: 'Steven Murawski'
tags: []
comments: true
draft: false
---

I was minding my own business, when Donovan Brown ([twitter](https://twitter.com/donovanbrown), [blog](https://www.donovanbrown.com/)) DM'd me on Microsoft Teams to ask for a second pair of eyes to troubleshoot a test case that was failing.

The tests were for a command in the [VSTeam](https://github.com/darquewarrior/vsteam) module. There were three tests in a [Context block](https://pester.dev/docs/commands/Context). Each test had a [Mock](https://pester.dev/docs/commands/Mock) inside of their [It block](https://pester.dev/docs/commands/It). The exact tests didn't matter so much, but I've included them below to give you an idea of what the structure looked like.

## Debugging the test

When we ran the tests, the third test in the block would fail by reporting that the mock of `Invoke-RestMethod` was called twice. To debug the test, we did a few things:

* First, we shuffled them around and it was always the second or third test that failed with the same error
* We looked at the function definitions and any functions those called and talked through the logic
* We added logging in the mock to see how often it was being called as the tests ran (it was firing once per test)
* We changed the mock definition from inside the `It` to the `Context`
* We looked at the documentation for [Assert-MockCalled](https://pester.dev/docs/commands/assert-mockcalled/)

After none of that seemed to shed any new light, I suggested putting a `Write-Host "Assert-Mock with $uri` line in the parameter filter of the `Assert-MockCalled` of the failing test. This led to us seeing each test evaluated all the previous `Invoke-RestMethod` calls within the `Context`.

## Afterward

Once we had a good understanding of what was happening, we looked deeper into the [docs](https://pester.dev) and [GitHub project](https://github.com/pester/pester) to see if we could find anything that documented that behavior.

There was nothing in [about_Mocking](https://github.com/pester/Pester/blob/4.10.1/en-US/about_Mocking.help.txt) and the docs are in transition from the GitHub wiki to [pester.dev](https://pester.dev).  In the wiki, sections about [Describe](https://github.com/pester/Pester/wiki/Describe) and [Context](https://github.com/pester/Pester/wiki/Context) both state that when they go out of scope, any mocks inside are cleaned up.  There's no such claim in the [It](https://github.com/pester/Pester/wiki/It) scope. The help for `Assert-MockCalled` and related `Assert-`'s did not shed any light either. 

I did find a line at the start of the help for [Mock](https://pester.dev/docs/commands/Mock) that was definitive.

> This creates new behavior for any existing command within the scope of a Describe or Context block.

### Results

There are two main results from our experience.

First, pair programming (more specifically, pair debugging) is a wonderful thing.  We did this over Microsoft Teams while self-quarantining in our own residences (we also live in different states). Getting a fresh pair of eyes on a problem can help as the primary developer walks the new person through what is happening, revealing assumptions and uncovering hidden dependencies.

Second, Pester needs updated documentation on the topic.  [We aren't the only ones who have hit this issue - there's an issue filed from the end of 2019 as well.](https://github.com/pester/Pester/issues/1416) It's an open source project maintained by volunteers, so if you've got some spare time, maybe this would be a great way to get involved with a project that fills a real need in the PowerShell community.

## Reference

The context block we were troubleshooting looked like:

```powershell
Context 'Add-VSTeamAccessControlEntry' {

  Set-VSTeamDefaultProject -Project Testing

  It 'By SecurityNamespaceId Should return ACEs' {
    Mock Invoke-RestMethod {
        return $accessControlEntryResult
    }

    Add-VSTeamAccessControlEntry -SecurityNamespaceId 5a27515b-ccd7-42c9-84f1-54c998f03866 -Descriptor abc -Token xyz -AllowMask 12 -DenyMask 15

    Assert-MockCalled Invoke-RestMethod -Exactly 1 -ParameterFilter {
      $Uri -like "https://dev.azure.com/test/_apis/accesscontrolentries/5a27515b-ccd7-42c9-84f1-54c998f03866*" -and
      $Uri -like "*api-version=$([VSTeamVersions]::Core)*" -and
      $Body -like "*`"token`": `"xyz`",*" -and
      $Body -like "*`"descriptor`": `"abc`",*" -and
      $Body -like "*`"allow`": 12,*" -and
      $Body -like "*`"deny`": 15,*" -and
      $ContentType -eq "application/json" -and
      $Method -eq "Post"
    }
  }

  It 'By SecurityNamespace Should return ACEs' {
    Mock Get-VSTeamSecurityNamespace { return $securityNamespaceObject }
    Mock Invoke-RestMethod { return $accessControlEntryResult } -Verifiable

    $securityNamespace = Get-VSTeamSecurityNamespace -Id "58450c49-b02d-465a-ab12-59ae512d6531"
    Add-VSTeamAccessControlEntry -SecurityNamespace $securityNamespace -Descriptor abc -Token xyz -AllowMask 12 -DenyMask 15

    Assert-MockCalled Invoke-RestMethod -Exactly 1 -ParameterFilter {
      $Uri -like "https://dev.azure.com/test/_apis/accesscontrolentries/58450c49-b02d-465a-ab12-59ae512d6531*" -and
      $Uri -like "*api-version=$([VSTeamVersions]::Core)*" -and
      $Body -like "*`"token`": `"xyz`",*" -and
      $Body -like "*`"descriptor`": `"abc`",*" -and
      $Body -like "*`"allow`": 12,*" -and
      $Body -like "*`"deny`": 15,*" -and
      $ContentType -eq "application/json" -and
      $Method -eq "Post"
    }
  }

  It 'By SecurityNamespace (pipeline) Should return ACEs' {
    Mock Get-VSTeamSecurityNamespace { return $securityNamespaceObject }
    Mock Invoke-RestMethod { return $accessControlEntryResult } -Verifiable

    Get-VSTeamSecurityNamespace -Id "58450c49-b02d-465a-ab12-59ae512d6531" | 
      Add-VSTeamAccessControlEntry -Descriptor abc -Token xyz -AllowMask 12 -DenyMask 15

    Assert-MockCalled Invoke-RestMethod -Exactly 1 -ParameterFilter {
      $Uri -like "https://dev.azure.com/test/_apis/accesscontrolentries/58450c49-b02d-465a-ab12-59ae512d6531*" -and
      $Uri -like "*api-version=$([VSTeamVersions]::Core)*" -and
      $Body -like "*`"token`": `"xyz`",*" -and
      $Body -like "*`"descriptor`": `"abc`",*" -and
      $Body -like "*`"allow`": 12,*" -and
      $Body -like "*`"deny`": 15,*" -and
      $ContentType -eq "application/json" -and
      $Method -eq "Post"
    }
  }
}
```
