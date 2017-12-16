---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2015-01-01T00:00:00Z
tags: []
title: My 2014 EOL and the 2015 Launch

---

2014 has been a phenomenal year and 2015 is shaping up to be as good, if not better!




## 2014





#### Starting the year with Stack Exchange, DSC, and SQL Server





2014 started with a lot of work around Desired State Configuration.  At Stack Exchange, there was a data center move looming up ahead as our colo facility at the time was unable to renew their lease.  One of the challenges we faced was getting our DR facility up to parity (on a service level) with our primary data center.  So, as part of that process, I spent a good bit of time building out DSC configurations for our basic service setup to speed the process of provisioning new nodes. 




We were also in the SQL Server early adoption program, and <a href="">Nick Craver</a> and I spent a good bit of time playing with the configuration and setup of replicas, as well as testing the upgrades between various pre-release builds.




#### Data Center Moves and Failover Drills





As winter gave way to spring, our data center move grew closer and we had to start testing our capacity in our Oregon data center.  We started doing some read-only tests (basically switching DNS to point to the IPs in the Oregon data center).  Each test found a bit of the infrastructure or application code that wasn't quite ready or tolerant of read-only.  After a number of these tests, we had a pretty high degree of confidence in our ability to move data centers.  The process was still pretty manual and lacked a bit of documentation, but it didn't fill us with fear at the prospect of failing over.  We had a power outage in our primary data center that forced us to do even more of a "failover drill" in the middle of the night.




Then came the data center move itself.  The data center move went relatively well.  Since my team was all in one place at the same time, after the bulk of the move is when I gave my notice that I would be leaving to join Chef.  My colleagues at Stack Exchange were awesomely gracious about my leaving (almost to the point that I was worried they were glad I was going - just kidding) and I can't speak highly enough of my teammates, the work we did (and they continue to do), and how they continue to deliver awesome service to the interwebs.




#### Joining Chef





In mid-July, I joined Chef.  It was a whirlwind of learning new tech, meeting new teammates, and figuring out what my role as a Technical Community Manager meant.




#### Conferences, conferences, and more conferences





I went to a few conferences earlier in the year - PowerShell Summit NA, Cascadia IT Conference, ForenSecure, and TechEd NA.  After joining Chef, I continued and ramped up pace with conferences and travel.  I was at TechMentor in Redmond, Velocity in NYC, helped teach a Windows Fundamentals class in London, PowerShell Summit EU in Amsterdam, Chef Summit in Seattle, Chef Summit in London, and TechEd in Barcelona.  I also taught a Windows Fundamentals class in L.A. in December.




## 2015





2014 ended with a few weeks of PTO (paid time off) around Christmas and the New Year.  On New Year's Day, I was honored and humbled to again be awarded as a Microsoft MVP in PowerShell.




#### Re-org'd and Re-classified





The first full week of 2015 saw a migration of Chef employees to Seattle for Chef Rally 2015.  At this event, we recapped the last year and discussed plans for this year.  Part of that planning was the complete reorganization of my team.  The Community team at Chef included Community Managers, open source developers, and our trainers.  We covered conferences, held public trainings and hack days, and got Supermarket (our community cookbooks site) shipped.  However as the team and scope grew, it began to overlap with what other parts of the organization were doing.  Marketing was also covering conferences, solutions architects and consultants were delivering training, and many of our employees pitched in on our open source projects and cookbooks.  




So, my team was disbanded (or I like to think of it as having the former Community team infiltrating other parts of the organization, fueling our love of and engagement with our community).  The Community team was relabled the Community Engineering team and tasked with enagaing with our community through mailing lists, IRC, GitHub, and other means and making sure our open source efforts move forward.  We've recently been instituting policies and governance to get more of our community "commit bits" (or the ability to commit to our core repositories) and providing a path for our community to help guide where Chef's software goes.  The Community Engineering team will be a vital part of interacting with those engaged members of our community who will help maintain the Chef project.




With the change from the Community team to the Community Engineering team, I'm no longer a Technical Community Manager.  I'm now a Software Development Engineer on the Community Engineering team.  So, does that mean that I'll stop evangelizing Chef, going to conferences, and teaching?  Not a chance.  It is the Community Engineering team after all and I'll keep working to make our community broader and more inclusive and easier to participate in.  




This switch (both to a Community Engineering team and involving more community maintainers) starts with strong fundamentals in managing open source projects, so this change will take a bit of time - but you will see a change in how quickly issues are responded to, pull requests reviewed (and hopefully merged), and how artifacts are shipped.




#### Coming up





As I said earlier, I'm still going to be active in the community and this is what I've got on the schedule for this year already..
* Chef Fundamentals on Windows class in Seattle, WA, in February
* MVP Open Days in Malvern, PA, in March
* ChefConf in Santa Clara, CA, at the end of March/beginning of April
* PowerShell Summit NA in Charlotte, NC in April
* Ignite in Chicago, IL, in May
* PowerShell Summit E in Stockholm, Sweden, in September




There are likely going to be a few more things added to that list as time goes (like a DSC Camp with Don Jones and the Chef Community Summit).




And I'll also be trying to make as many user groups as I can, whereever they may be.




It's going to be a fun year!

