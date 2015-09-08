---
layout: post
title: "There Is A New Windows Server, They Call Him Nano"
date: 2015-04-10 20:19
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


After the announcement of Nano there is definitely a more anti-Nano/Core/MinShell feeling in the Windows IT Pro community and I understand the root of that angst.  That said, Nano is the direction we need to head.  The introduction of Server Core heralded this back with Server 2008.  We had Core's reach extended in Server 2008 R2.  Server 2012 made Core no longer a life-choice and allowed to add the GUI (well, at least if you started, went down to Core, then came back up..).  We also saw the introduction of MinShell - a last gasp attempt to make admin tools available, while still removing the offending explorer/iexplorer patch magnet.




## Wait For Me!





The direction of Windows Server has been clear.  While other product groups and third parties may be slow on the uptake, they are shoehorning themselves (and the customers who rely on them) into the legacy space.  I've deployed production workloads on each version of Server Core since it existed.  It was challenging, but it was worth learning and understanding the limits and challenges.




## Ah RDP, How I Rely On Thee





Windows Server with a GUI is likely not going away for some time - heck there are still a ton of folks with lots of Server 2003 out there.  But that shouldn't hold Microsoft back from dealing with today's business realities.  If Windows Server 2003 met all our business needs, who cares what comes in later operating systems?  If you are happy with full UI Server 2008 R2 where it stands, no one is strongarming you to move to MinShell on 2012R2.  If 2016 ships with a default of MinShell and no full UI option by default, DISM yourself up a UI and move on.




## Full Speed Ahead, Damn The Torpedoes!





Nano is the future for Windows server.  The break with backward compatibility needs to happen for Windows Server to be REALLY relevant in the cloud space.  And the cloud space (whether public, private, or hybrid) is what really matters today and probably for the next few years.




## What?!? There Is A Change In IT?  Say It Ain't So..





But, you say, "my people can't operate servers like that" or "we can't get training on that" or "we are too busy fighting fires to learn anything new".  Well, nothing says you have a right to stay successful.  And at the end of the day, each one of us is personally responsible for our own careers.  The patterns of IT operations are changing and if Windows Server and Windows Server admins don't meet the needs of the business...  Individual IT Pro's need to step up and acknowledge their responsibility for maintaining and extending their skills.  And if their employer doesn't enable that, there are people looking for skilled operations folks. (Yes, leaving an employer is not a cavalier decision by any means - but we need to own the choice.)




## Oh Yeah, I said "DevOps" - Deal With It





Yep, that pattern (some might call it DevOps) is still in the minority, but the business impact is being seen, studied, and talked about.  Shadow IT is growing up in organizations to get around stagnant IT departments.  PAAS, IAAS, and SAAS offerings provide business with the capability to extend their IT capacity with a credit card.  Yes there are a host of challenges, like data security and confidentiality and integrations to existing systems, but the offerings can be compelling enough to start those conversations.




## There Are Other Operating Systems?  Crazy Talk.





And there are organizations, large enterprises - not just webops shops, embracing these changes and finding success there.  Many of these efforts have excluded their Microsoft stack and much of the tooling in this space ignores or has limited functionatity with Microsoft products because their design and implementation actively hinder high velocity operations.




## Let's Rub Some Cloud On IT





As much as it pains me to say it, cloud is a great pattern for dealing with infrastructure.  Windows Server (currently) isn't a great cloud OS.  Nano is the start of fixing that.  Lightweight for minimal update surface, rapid deployment, and reduced hardware requirements.  Core features provide a great host for containers and VMs, allowing for more rapid iteration of environmental configurations and application deployment.  Nano stands to provide a great platform for application deployment (.NET or otherwise).  Low overhead means more resources for application workloads.  Easy and speed of deploying new instances or new containers enables effective scale-out.  No UI to fumble around in encourages configuration management (DSC/Chef/Other), so deployments are repeatable, consistent and self healing.




## You Got Some Enterprise In My DevOps





So what about SQL Server? Well, I ran SQL Server on Core and MinShell and it worked pretty darn well (MinShell was at Stack Exchange).  Will it support Nano?  Who knows?  SQL Server really hasn't shown much interest in the web operations space or devops in general.  We were very close to switching to Postgres at Stack, due to the outrageous Enterprise pricing.  We had a project that was in flight to port data access to Postgres and it was a very realistic option.  And guess what?  I can run it on Linux on a very lightweight, minimal operating system and for the price of all that licensing (Server licensing was almost negligble), I could hire a couple of experienced Postgres experts to tune and maintain the crap out of it.




## Go Forth And Make More Awesome





I'm so thankful that Microsoft is moving this direction with Server.  It has my circle of people, many of whom have worked very hard to stay out of Microsoft admininstration, excited and energized about Windows Server and Azure.




I know that I'm the current miniority, but in my day job we have a number of large customers who have substantial Windows deployments who are working to adopt a devops workflow and culture.  This direction will only help encourage them.




If you want to cross the divide and enjoy IT operations, minimize firefighting, and expand your horizons, let me know where I can help.

