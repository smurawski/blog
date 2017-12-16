---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2017-06-30T00:00:00Z
tags: []
title: Pruning Branches From Your Source Tree

---

[Shannon Dunn](https:/twitter.com/sdunn_dev) reached out with a question about moving to VSTS with Git repositories.

## The Scenario

Two groups of developers are working on one code base.  One group is an internal dev team and the other is a contract dev group.

Shannon acknowledges that this is really more of a process challenge than a technical one.

> Over my many years I always find its the "right path" through tech that is the harder thing to know...not the tech itself.

And Shannon has laudable goals,

> Like I said, like many devs in smaller shops, I don't have a background in the OPS side of DevOps. I had my source control management system and worried about my team. Now that we have n number of outside shops working with us, and we are trying to get to automated and better automated testing deployments, its harder to know the right path on these things. 

### Criteria

#### The source system should

* allow all developers to stay current with the latest accepted changes.
* allow for easy identification of the current deployment (and past deployments) 

#### The source system should not

* allow the contract team to push changes directly to `master`
* prevent internal developers from easily delivering critical fixes

### Proposed structure

Shannon proposed, based on previous experience and suggestions from others in the industry, a structure with several branches.  I'll simplify it a bit for this post, but the concept is the same with three long running branches or thirty.

#### Branches

* `master`
    * this is the currently shipped application
    * internal dev team should be able to push directly in an emergency
    * contract team should not be able to push to this branch
* `internal_dev`
    * this is where the main development efforts for the internal team would live
    * contract team should not be able to push to this branch
    * changes from this branch would be merged to `master` 
* `contract_dev`
    * this is where the main development efforts for the contract team would live
    * internal devs would have access, but only use it to bring things back up to meet the current versions

### Challenges

This type of scenario can be set up ([and there's good documentation on the configuration settings](https://www.visualstudio.com/en-us/docs/git/branch-policies)), but that doesn't control other branches (git makes it easy to create branches as long as you have rights to the repo).  We could go down the path of ACL'ing the heck out of things, but that adds to the maintenance overhead, troubleshooting access, and cleaning up when someone does something unexpected.

It also doesn't address Shannon's question about workflow.

> How do I ensure that at the end of a launch that every branch is updated with the latest code?

With three main branches, this becomes a major pain point.  Identifying a point to refresh `contract_dev` from `master` isn't too hard, but what about syncing changes between `contract_dev` and `internal_dev`.  We would have to identify times to do this (perhaps the end of a sprint), but now we have to add an integration period.  These integration points are more painful the longer the branches diverge and a two or three week sprint with two teams can send the codebase in a lot of different directions.

## My Recommendation

I suggest using a set of long lived branches that look like

* `master`
    * We tag each release of the application
    * We protect the branch so no one can directly push to it (All changes through pull requests or PRs)
    * We require that all PRs be reviewed by someone on the internal developer team before merge to `master`.

Yep, just one long lived branch.

Rather than try to ACL off the world and constrain folks to a proscriptive flow across a number of branches, we should draw back our lines of defense to the most critical point - the `master` branch.  We want the freshest eyes looking at the smallest changes possible when they go into our `master` branch.  Rather than dealing with integration pain at the end of a sprint when moving large parts of different branches around, we deal with smaller integration problems daily.

So, where do we get dev builds and where do developers work from?

### The Workflow

This is just an abbreviated version of my takeaways from the book [Continuous Delivery](http://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912) and agumented by my work both in internal and open source projects.  I heartily recommend reading Continuous Delivery for a deeper treatment of the topic.

#### Working locally

Each developer (internal or contractor) locally clones a copy of the repository.  Each developer creates a branch for their immediate work item.  I suggest using a naming convention for the branches that identifies the work item and developer (can assist in clean up of straggling branches).

Each work item should be less than a day or two's worth of work.  If an item is bigger than that, break it up into smaller units that can be delivered individually.  If this is going to have an impact on existing functionality, it's worth adding feature flags to hide the new development until it is ready to be tested/used.  We want small units of work so we can get them merged into `master` in a timely manner.

Developers are responsible for making sure their branches are close to `master`.  The longer a developer works on their branch, the more `master` is going to change.  Git will let you `fetch` or `pull` the changes from `master` and `rebase` your work (changing the starting point for your commits to the current state of `master`).  Before creating a pull request, it is the developer's job to make sure they are rebased on the latest version of `master` and that things compile, lints pass, tests pass, and so on.

Then, because we are shooting for small units of change, the developer can create a pull request against `master` and one of the core team of internal developers will need to review and sign off on (approve) the merge to `master`.

This moves the integration pain of two largely different branches to many more integrations of much smaller differences.  The smaller the changeset, the more likely you'll experience a successful merge.

### Working on the central repo

As PRs come in, the group of internal developers who are approvers of the workflow will need to promptly review changes (you do not want a large outstanding number of pull requests sitting for long periods of time).  This may feel intrusive to folks who "just want to code".  Having more, smaller changes to approve actually make it easier to really review the change (rather than "Looks good to me").  And the longer pull requests sit, the more likely they'll need to be updated (rebased) before they can be merged as other code will come in that may conflict with the proposed changes in the pull request.

*NOTE:* I mention environment names like `dev` or `staging` and `production`.  Feel free to replace those with the appropriate names for your environment or add the additional environments you have.  E.g. you could have a `dev`, `staging`, `uat`, and `production`.  That is all fine, so long as you've automated the promotion of build artifacts between them.

We can use CI/CD system to build `master` on every merge (and build and run tests on every PR).  A successful build will produce an artifact.  That could be a zip file, a WebDeploy package, or whatever.  That artifact is what can be deployed to a dev or staging environment.  We can then see the combined efforts of the internal and contract teams, just as it would be in production.

When it is time to release, we can `tag` the commit on `master` and promote the appropriate versioned artifact to production.

This provides us with a tag that represents the source code for a particular release, which is related to the artifact that is created from the build. The artifact is what is promoted from your development or staging (or whatever name) to production.

### Circling back

Does the recommended workflow meet the requirements Shannon set out?  Let's see - 

- [X] All developers should stay current with latest changes - developers can pull changes from master and rebase at any point.
- [X] Easily identify current (and previous) production releases - tags and named artifacts identify released builds.
- [X] Contract developers cannot push directly to `master` - protecting the branch in VSTS prevents any direct push to master.
- [X] Does not prevent internal developers from pushing critical fixes - sort of, as long as the developer is an "approver",  she will be able to approve her own PRs and create a build.

With respect to the last point, even emergency fixes should be built and deployed through the pipeline.  You may want to put some flags in the build/release process to expedite some of the longer tests, but it may be worth focusing on the test runs to really optimize them or scale them out to speed things along.

This also means we cannot leave `master` broken.  If the build breaks, all merges except code to fix the break are on hold.  We need to keep `master`'s build green so that we can build a release at any point in time.

I think though, this workflow meets the spirit of Shannon's goals and, more broadly, is a good foundation for most development pipelines.
