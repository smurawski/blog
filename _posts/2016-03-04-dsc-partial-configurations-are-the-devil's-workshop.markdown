---
layout: post
title: "DSC Partial Configurations are the Devil's Workshop"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

Windows Management Framework 5 introduces Partial Configurations.  Partial configurations are basically named containers in the Local Configuration Manager (LCM) that can pull from different sources.  Each container can have certain resources reserved for it and only it.  Then when the LCM is ready to validate the current configuration, it will put together all the partial configurations into one configuration to validate on the node.

### Let's make configurations less reliable so your teams still don't have to talk

So, this sounds great!  Let's have the SQL team use DSC to manage the SQL bits, while the OS team manages the base OS.  And of course, our web team will get the IIS configurations.

Partial configurations will let us lock things down and say - "Only you can prevent SQL configuration drift" via xSqlServer resources.  And the web team will be given dominion over all things xWebAdministration.

Until that fateful day when the DBA's want to run Reporting Services.  And now they want to use xWebAdministration to manage their IIS components.  Since xWebAdministration is barred from them, those enterprising DBAs turn to cWebAdministration.  

Now we get to audit time and all the web sites are inventoried, but those pesky reporting sites didn't follow all the configuration standards!!  Oh noes!!

#### Or maybe it's not that complicated

Maybe it's as simple as the OS team setting the default power plan to Balanced for the box via my PowerPlan resource.  But the SQL Server team doesn't want that, they need High Performance.  So they use xSQLServerPowerPlan to set it to high performance.

Now, on every configuration check, the power plan will have "drifted" to whichever ran last.  The node will be continually "fixing" itself to whichever resource runs last.

#### Or yet another problem

Maybe we didn't get around to restricting who can use what resources and both teams (SQL And OS) use my resource to set the power plan.  Neither team will see an error until that configuration is applied to the node, which will then just not apply any configuration, as the full MOF will be invalid - duplicate keys and all.

### Partial Configs are not the answer

This collaboration really belongs earlier in the process.  Teams can "own" different parts of the configuration, but all teams should have some visibility in to the process.  Being able to bring all those settings together ahead of time allow you to build a testing and validation pipeline that can look for those conflicts and surfaces where two teams may use conflicting resources or settings.

So, if you are thinking about partial configurations, step away slowly and think about how you can maintain independent control, but bring configuration together sooner.