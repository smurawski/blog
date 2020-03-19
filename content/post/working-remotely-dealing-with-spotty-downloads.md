---
title: "Working Remotely - Dealing With Spotty Downloads"
date: 2020-03-19T16:46:53-05:00
author: 'Steven Murawski'
tags: []
comments: true
draft: false
---

Today I was trying to download an 11 GB zipped folder from a service at work over VPN. From the browser, the URI I had for the download would make it about 1 or 2 GB into the download and fail. It would keep failing for a while, even if I "resumed" the download.

Then, I remembered dealing with this while working between remote sites in a previous role. I used the [BitsTransfer PowerShell module](https://docs.microsoft.com/powershell/module/bitstransfer/Get-BitsTransfer?view=win10-ps&WT.mc_id=social-blog-stmuraws).  This module uses the Background Intelligent Transfer service to manage uploads or downloads that may be disconnected, suspended, and resumed. (It's what Windows Update uses to download updates for example.)

Normally, the BITS Transfer commands can handle a bit of interruption and restart, but I'm impatient. I ended up writing a short script in the console to start the transfer, check on the job every couple of minutes and restart it if the job if it's in the `TransientError` state.

```powershell
$Job = Start-BitsTransfer -Source $MyURLToSomethingOverVPN -Destination video.zip -Dynamic -Asynchronous -TransferPolicy Always -Priority Foreground

$JobState = (Get-BitsTransfer -JobId $Job.Id).jobstate

while (('TransientError', 'Transferring') -contains $t) {
  if ($t -eq 'TransientError') {
    Get-bitstransfer | resume-bitstransfer -async
  }

  write-host 'Taking a short nap'
  start-sleep -Seconds 120

  write-host 'Taking a look at the job...'
  $t = (Get-BitsTransfer -JobId $Job.Id).jobstate
  write-host "The job is $t"
}
```

If you haven't used it in a while and need to manage large downloads over uncertain network conditions, [check out the documentation and give it a try](https://docs.microsoft.com/powershell/module/bitstransfer/Get-BitsTransfer?view=win10-ps&WT.mc_id=social-blog-stmuraws) 
