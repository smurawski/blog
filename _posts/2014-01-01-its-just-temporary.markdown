---
layout: post
title: "It's Just Temporary..."
date: 2014-01-30 18:23
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


><p style="margin-left:0px; margin-right:0px">[Vizzini](http://www.imdb.com/name/nm0001728/?ref_=tt_trv_qu): HE DIDN'T FALL? INCONCEIVABLE.


[Inigo Montoya](http://www.imdb.com/name/nm0001597/?ref_=tt_trv_qu): You keep using that word. I do not think it means what you think it means.




## It does not mean what we think it means...



Many of us have seen [The Princess Bride](http://www.imdb.com/title/tt0093779/?ref_=ttqt_qt_tt)&nbsp;and have heard Inigo's response to Vizzini after Vizzini has excessively used the work "inconceivable". &nbsp;In the systems administration world, we suffer the same lack of clarity around the word "temporary".


Usually this comes from the desire to get a task done to either satisfy a persistent requester or due to a perceived urgency for the task. &nbsp;We phrase it harmless enough,


*   I'll just quickly bang out a script.
*   I'll just fix the error manually this time.
*   I'll just RDP to the server this one time.
*   I'll just throw dev and production on this box for now.
*   I'll just ...

You get the picture. &nbsp;The justification for all this is that it is "only temporary". &nbsp;


## And time expands..



These actions taken with the perception that the situation is "only temporary" fails to take into account our experience that either they or their effects persist long into the future. &nbsp;


Taking a page out of my personal experience, that "temporary server" stood up to deal with a failure to plan for a new service roll out was still in production months later, and the source of a number of additional bugs in our bug tracker. &nbsp;The time working on issues in that environment more than overcame what time would have needed to be invested to deal with the deployment properly.


## Covering hidden peril



"Temporary" fixes tend to hide some pretty major consequences.


Another side effect of this things done "temporarily" are very often undocumented - since they won't be in place very long. &nbsp;This can lead to sending your co-workers down rabbit holes trying to figure out unexpected behavior.


The final side effect I'm going to mention is specifically about scripting. &nbsp;"Temporary" or "throw-away" scripts tend not to make it into source control, usually don't have any help or documentation, and usually don't have any tests or error handling.


# What can I do about it?



Man up (or should I say Person up?). &nbsp;If you have a task, do it like it's going to be around for a while. &nbsp;


**Follow **your organization's **conventions **and **guidance**.&nbsp;


Be **realistic **about **what **you can accomplish and by **when**.


Focus on the minimum **viable **product. &nbsp;Viable is key here, it needs to satisfy the use case. &nbsp;Viable means that it's supportable/sustainable.


If you won't be able to meet the target for when that task is done,** let the stakeholders know - as early as possible**. &nbsp;You aren't doing anyone any favors by not doing it right. &nbsp;In fact, you are probably doing them a disservice when the consequences of the quick fix come due.


And, in the case you need to do a quick fix, throw-away script, or one-off server build, file bugs in your bug tracker to fix it and/or put deadlines on your/team/organization calendar(s) for remediation of temporary work. &nbsp;Make it visible.

