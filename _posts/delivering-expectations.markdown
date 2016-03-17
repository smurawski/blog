---
layout: post
title: "Delivering Expectations"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

### TL;DR;
 
Microsoft (specifically the PowerShell team in this case) is speeding up.  We are seeing new features faster and more frequently.  This comes at a cost (when compared to the old three year ship cycle).  Documentation is rougher (or missing), some things break, and some do not work as expected.  People/hours are finite and there is only so much the PowerShell team can do in a ship cycle, but we have more ship cycles so we can see things improved faster.

Breakage stinks, but software is written by humans and we are not perfect.  It is fully on the PowerShell team to prevent when possible and fix (and put some tests in place to prevent the regression) and release updates when a breakage does happen.  This is a growing pain with any team looking to ship software more frequently.

The state of PowerShell doesn't rest fully on the PowerShell team though.  We now have a better ability to [help improve the documentation](https://github.com/PowerShell/PowerShell-Docs) AND [the test suite PowerShell is validated against](https://github.com/PowerShell/PowerShell-Tests) (currently only by filing issues, but hopefully they'll take PR's in the future).

If this is scary to you or you aren't ready to take some ownership for your use of newer features, don't do it. Don't install the preview builds.  Don't write class based resources.  Don't use the awesome remote debugging.  Stick with WMF 4 compatible stuff or WMF 4.  There is much less chance of things breaking that way.  Just remember, things will be moving forward without your eyes or your feedback.

*DISCLAIMER:  This is my view from the outside as a customer and NOT and "official" explanation or history.*

## It's Going Faster!

Since Server 2012 R2 / PowerShell 4 RTM'd (in october 2013), we've had seven (count 'em - there's seven) releases of the Windows Management Framework 5 - and Server 20126 hasn't even shipped yet.

*NOTE: the Windows Management Framework is the ship vehicle for PowerShell, but also can include other things like WinRM and BITS updates.*

## Why So Many?

One of the reasons we've seen so many releases of the Windows Management Framework (WMF) is that the PowerShell team wants our feedback earlier.

### In The Pre-Cloud Days

In the past, they've released several previews to select customers and MVPs who could then provide feedback before things shipped.  Since the WMF release was coordinated with the Server OS release, there were usually some hard time blocks which major feature changes had to be in by.

This meant if you weren't "in the club", you wouldn't get to see the early product or maybe you'd see one preview release, but things were pretty well set in stone by that point.

This was pretty helpful in terms of having more time to prepare documentation, run batteries of integration tests, and give those selected customers and MVPs chance to run those early releases to help look for bugs or missed features.

For most PowerShell users, this meant waiting until the Server OS shipped (and given the fact that a bunch of companies just recently got around to deploying Server 2008 R2, waiting until an organization's OS refresh) to be able to kick the tires in a meaningful, supported way.

Any bugs or feature requests based on what you found in the OS could be filed on Connect.  In just 3 to 5 short years time, you could see if maybe, just maybe, your feedback was taken.

### Entering the Cloud Focused Microsoft

We've started to see a shift in how Microsoft delivers software and the PowerShell team is on the leading edge of this with the Windows Management Framework.  Starting in May of 2014, they started shipping public previews of the WMF to everyone.  Now, everyone/anyone could weigh in early and often about the direction and feature set in the WMF and the PowerShell team now had a reasonable chance to act on feedback from the public at large.  Over the course of the WMF 5.0 preview process, there were 5 more preview builds, roughly quarterly (it was a bit more "roughly" at first).

This also meant that we, as the general public, got a glimpse of features that were in various states of completion.  This is both great and scary.

#### What's Great?

* What's coming? is no longer a black box
* Chance help shape new direction for PowerShell
* Test your existing workloads against the newer builds
  * You can be ready when it ships
  * Find and report bugs before it ships

#### What's Scary?

* Limited or no documentation
  * Features aren't done, so comprehensive documentation is hard to come by
* Had the "preview" warnings of "Do Not Install In Your Production Environment"
* Some things just did not work

## This Is The New "Normal"

Microsoft and the PowerShell team are picking up their release cadence and starting to deliver change on a faster pace.  There are growing pains associated with this process.  The PowerShell team will adapt to this and continue to improve.  It is our job as the PowerShell community to help that process.  

We can throw up our hands and say,"I miss the old days where we got a finished product".  If you've ever visited Connect (and now UserVoice), you'll know that wasn't true.  Every release of PowerShell and the WMF have had bugs, gaps in documentation, and missing capabilities.  With the increased release cadence and preview process, we see and feel these things more vividly than before since many things are "in progress".  Documentation lags since the feature has to be done(ish) to write the docs.  Integration tests lag for similar reasons.  This is part of the process of moving to Continuous Delivery.

## Yep, I Said It - Continuous Delivery

The PowerShell team, in my view, is on the path to Continuous Delivery.  Continuous Delivery is the process by which your main branch of code is always "releaseable".  I'll refer you to [the Continuous Delivery book by Jez Humble and David Farley](http://www.amazon.com/gp/product/B003YMNVC0) for a more in-depth discussion on the topic.

*NOTE: Continuous Delivery is NOT the same as Continuous Deployment.  In Continuous Deployment, every successful build is shipped.  Continuous Delivery's goal is every build is "ship-able".*

One of the core concepts in Continuous Delivery is to bring the pain forward and if it hurts, to do it often.  This forces you to focus on the hard parts of the process and find/create automation to deal with those pain points.

This frequent preview process has brought to light that there are gaps in test coverage (things like Get-Help and Get-Command appear to have some regressions in their behavior for example).  In response, we see the PowerShell team moving to an OSS test framework ([Pester](https://github.com/pester/pester)) and [starting to surface their test suite for PowerShell](https://github.com/powershell/powershell-tests).  Eventually (hopefully), they'll take pull-requests to the test suite, but for now we can see what they are checking and file issues for things that are missing.  Because documentation was slow to follow, we see [the documentation for PowerShell on GitHub](https://github.com/powershell/powershell-docs) and they do take pull-requests!

## Some Of This Is On Us

While the responsiblity for PowerShell resides within Microsoft, we the PowerShell Community own our experiences with PowerShell.  And for those areas in which we can publicy contribute, we have a previously unattainable level of access.  There are a [growning number of public projects in the PowerShell ecosystem driven by the PowerShell team](https://github.com/powershell) that we can contribute to and make a difference in our experience.  Find a documentation issue?  Submit a PR and get it fixed.  Find a command not behaving as expected? Hit UserVoice or the Issues page of the PowerShell-Tests project.  DSC resource acting funky?  Get a PR submitted and help improve it.

## If You Really Like The Way It Was

If you really want the same experience you had with PowerShell 1, 2, or 3, you could wait on installing WMF 5 or keep your usage to just the WMF 4 compatible stuff.  Wait a few more years, then come back to WMF 5.  No one is forcing you to use all of WMF 5.  By that point, I'll be on PowerShell 17 and reveling in my early adopter role, but the slow and steady path is still there.

The conflict comes when you want the new features faster, without any of the downsides.  I doubt that we'll ever be fully there, but I have faith that the PowerShell team will get closer to that reality.  We just need to give them time and some help and encouragement to get them there.
