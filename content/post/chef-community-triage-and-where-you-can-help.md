---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2015-03-01T00:00:00Z
tags: []
title: Chef Community Triage and Where You Can Help!

---

## Our Triage Process



The Community Engineering team, along with others in Chef engineering, have been working to triage issues that come in on our open source projects.


Part of that process includes identifying and categorizing issues and assigning them to milestones.&nbsp; Those milestones are general targets and do not, repeat, do not mean that they'll be addressed for any particular release.&nbsp; What the milestones do is provide a backlog to work from for those release points.&nbsp; At this time, we've got to three main milestones (the names of which are subject to change).&nbsp; They are "[Accepted Major](https://github.com/chef/chef/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Accepted%20Major%22)", "[Accepted Minor](https://github.com/chef/chef/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Accepted%20Minor%22)", and "[Help Wanted](https://github.com/chef/chef/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Help%20Wanted%22)".


The "Accepted Major" milestone is intended for features with breaking changes or with a very large scope.&nbsp;


"Accepted Minor" covers work that we, as Chef Engineering, want to implement or resolve - though PRs from the community for features or bugs with this milestone are welcome and encouraged!&nbsp;


The final milestone is "Help Wanted" which covers bugs or feature requests that we'd like to have implemented in Chef, but fall below the priority level for features or bugs in the previous two milestones.&nbsp;


Features or bugs can move between milestones (especially as feedback is added to existing issues or there are multiple reports or requests for a particular bug or feature).&nbsp;


The "Help Wanted" milestone is where we really want your help.&nbsp; If you have bugs or features you'd like to see that are in this milestone, we are counting on the community to help us deliver these.&nbsp; You can find all the[ "Help Wanted" milestone issues for the Chef project](https://github.com/chef/chef/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Help+Wanted%22) with a simple search.


## We Want Your Help



If you are looking for a place to start getting familiar with the Chef code base, fixing some bugs off that list can definitely be a great start.&nbsp; We've also been tagging issues with a relatively narrow scope or ones that we felt have basic implementations with the "Easy" label.&nbsp; There are a handful of issues that fall into the ["Easy" label and "Help Wanted" category](https://github.com/chef/chef/issues?q=is%3Aopen+is%3Aissue+label%3AEasy+milestone%3A%22Help+Wanted%22).


So, head on over to GitHub, fork the [Chef](https://github.com/chef/chef) project, and help us out with one of these issues:


*   [chef daemon mode not recognizing new json_attributes](https://github.com/chef/chef/issues/1632)&nbsp;&nbsp;
*   [cookbook upload fails if there is a brace in the cookbook path](https://github.com/chef/chef/issues/1625)
*   [git resource Errno::EIO on windows](https://github.com/chef/chef/issues/1622)
*   [Chef can't remove apt packages if they don't exist in a repo](https://github.com/chef/chef/issues/1600)
*   [OSX User provider blows up if group is removed before user](https://github.com/chef/chef/issues/1586)
*   [Debian service resource is not idempotent when priority is specified&nbsp;](https://github.com/chef/chef/issues/1517)

If you need help getting started, reach out to me or one of the other Community Engineering team on the [Chef Developer mailing list](http://lists.opscode.com/sympa/arc/chef-dev)&nbsp;or in IRC and we'll do what we can to help you get started.&nbsp; Or stop on in to [Chef Developer Office Hours](https://twitter.com/chefofficehours) - Monday's and Wednesday's at noon Pacific, 3 PM Eastern.

