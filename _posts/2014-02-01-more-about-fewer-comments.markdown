---
layout: post
title: "More About Fewer Comments"
date: 2014-02-18 14:30
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


[My last post about comments](http://stevenmurawski.com/powershell/2014/2/just-stop-with-the-comments-already) generated a bit of conversation on Twitter, which highlighted my failure to communicate a few points.


My dislike for comments is predicated on two main concerns. &nbsp;


First, if I find myself needing to leave a comment, I've accepted the fact that my code isn't clear enough to explain what it is doing. &nbsp;This could indicate a problem in my function or variable naming, or a need to refactor a dense block of code.&nbsp;


Second, I'm committing to my code base or script a comment that does not actually surface during any possible way during execution. &nbsp;As the script is updated, the chance that the comment will be updated to match the evolving functionality is very unlikely.


# Are Comments&nbsp;Help or Documentation?



First and most importantly.. comments used as a convention to deliver help (like PowerShell's Comment Based Help) are not what I'm referring to. &nbsp;Other comments that fall into this category are things like javadoc or xml comments in c#. &nbsp; Those use the comment syntax to deliver documentation end user of the code. &nbsp;Since these comments are used exposed (either at run time in the case of comment based help, or at document generation time), there visibility is higher and the chance of staying up to date are much more likely.

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@Jaykul](https://twitter.com/Jaykul) [@StevenMurawski](https://twitter.com/StevenMurawski) Help tells you how to use it. Comments tell you how/why it works. Reading commented code is the best way to learn.
&mdash; June Blender (@juneb_get_help) [February 14, 2014](https://twitter.com/juneb_get_help/statuses/434424757119766529)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


I totally agree with June's description of the difference between comments and help. &nbsp;What I disagree with are that comments **should** be used to describe how and why it works. &nbsp;This leads to the next topic.


# Should Comments Be Used To&nbsp;Understand Advanced Code?


 
   <blockquote class="twitter-tweet" data-conversation="none" data-cards="hidden" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) I respectfully disagree. How do you understand intent of advanced code w/o comments? e.g. [http://t.co/p312Fl0ECn](http://t.co/p312Fl0ECn) by [@Jaykul](https://twitter.com/Jaykul)
&mdash; June Blender (@juneb_get_help) [February 14, 2014](https://twitter.com/juneb_get_help/statuses/434399352069439488)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


There seems to be a feeling that code cannot be understood without comments. &nbsp;There are situations where a comment (or comments) can be helpful in clarifying why a choice was made, but a comment should be the LAST resort. &nbsp;


Let's look at the example June ([@juneb_get_help](https://twitter.com/juneb_get_help))provided. &nbsp;(I'm not picking on June, she's a great person and her work and support in the PowerShell community has been much appreciated over the years. &nbsp;She was just kind enough to disagree with me and share her thoughts on Twitter.)


She referenced one of Joel's ([@Jaykul](https://twitter.com/jaykul)) scripts in support of his IRC bot.&nbsp;

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

Not all code requires comments to be instructive [@juneb_get_help](https://twitter.com/juneb_get_help), as [@StevenMurawski](https://twitter.com/StevenMurawski) says. Was my module confusing for missing comments?
&mdash; Joel Bennett (@Jaykul) [February 14, 2014](https://twitter.com/Jaykul/statuses/434425894858280960)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


As an option to comments, I'd suggest a bit of refactoring to clarify intent. &nbsp;For example,&nbsp;breaking out some of his parameter token parsing logic into separate functions, rather than having one longer block of code. &nbsp;(I'm not picking on Joel's code here, I've done the same previously and I'm still working to fix public examples that I have. &nbsp;His is just the example that June pointed out.)

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@Jaykul](https://twitter.com/Jaykul) [@StevenMurawski](https://twitter.com/StevenMurawski) Joel, it&#39;s a great example of a complex script that requires comments to understand the intent and coding choices.
&mdash; June Blender (@juneb_get_help) [February 17, 2014](https://twitter.com/juneb_get_help/statuses/435488166032580609)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


Rather than littering already dense code with additional comments, I'd start to pull blocks of code out into separate functions.

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) [@jsonmez](https://twitter.com/jsonmez) I think MOST code is pretty straightforward. Where comments help are in understanding complex decision trees.
&mdash; Kendal Van Dyke (@SQLDBA) [February 17, 2014](https://twitter.com/SQLDBA/statuses/435491247143538688)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) [@jsonmez](https://twitter.com/jsonmez) ...and in understanding complex algorithms.
&mdash; Kendal Van Dyke (@SQLDBA) [February 17, 2014](https://twitter.com/SQLDBA/statuses/435491440878432256)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


As Kendal (<a href="https://twitter.com/sqldba" data-cke-saved-href="https://twitter.com/sqldba" target="_blank">@SQLDBA</a>) points out, complex decision trees can be difficult to follow in a large block of code or complex algorithms. &nbsp;I definitely agree that following complex decision trees warrants some additional help. &nbsp;Here again, though, I'll look to refactor first if I felt the need for comments and clarification.

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) [@juneb_get_help](https://twitter.com/juneb_get_help) [@Jaykul](https://twitter.com/Jaykul) One more perspective - comments also good for explaining why you DIDN&#39;T do something.
&mdash; Kendal Van Dyke (@SQLDBA) [February 17, 2014](https://twitter.com/SQLDBA/statuses/435492870074552321)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


Kendal also raises a valid point about commenting why you didn't do something. &nbsp;Comments should be an exceptional case, for exceptional situations. &nbsp;This can definitely fall into that case.


# What Is Considered A Comment?


 
   <blockquote class="twitter-tweet" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) so are you saying no to [#region](https://twitter.com/search?q=%23region&amp;src=hash) [#endregion](https://twitter.com/search?q=%23endregion&amp;src=hash)? Those blocs are amazing?
&mdash; Jason Morgan (@RJasonMorgan) [February 14, 2014](https://twitter.com/RJasonMorgan/statuses/434404443505713152)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


I've got mixed opinions about the region/endregion statements. &nbsp;I find them useful for grouping sets of functions. &nbsp;Once I see region/endregion inside a function, I start getting a bit nervous. &nbsp;That segment cries out for refactoring.&nbsp;

 
   <blockquote class="twitter-tweet" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) Also need to ask, Write-Verbose and Write-debug, good code uses them as comments. Does that fall into your bad list?
&mdash; Jason Morgan (@RJasonMorgan) [February 14, 2014](https://twitter.com/RJasonMorgan/statuses/434404882540290048)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) [@RJasonMorgan](https://twitter.com/RJasonMorgan) I like Write-Verbose, because all you have to do is append the -Verbose common switch parameter :)
&mdash; Trevor Sullivan (@pcgeek86) [February 14, 2014](https://twitter.com/pcgeek86/statuses/434405519655063552)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@StevenMurawski](https://twitter.com/StevenMurawski) [@RJasonMorgan](https://twitter.com/RJasonMorgan) Agreed - came into the middle of the conversation. Verbose and Debug should not be code comments.
&mdash; Trevor Sullivan (@pcgeek86) [February 14, 2014](https://twitter.com/pcgeek86/statuses/434406745973075971)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@pcgeek86](https://twitter.com/pcgeek86) [@StevenMurawski](https://twitter.com/StevenMurawski) they are cetainly a type of comment, in that they announce what the code is doing in plain english.
&mdash; Jason Morgan (@RJasonMorgan) [February 14, 2014](https://twitter.com/RJasonMorgan/statuses/434407144725557249)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@RJasonMorgan](https://twitter.com/RJasonMorgan) [@StevenMurawski](https://twitter.com/StevenMurawski) [@pcgeek86](https://twitter.com/pcgeek86) Verbose and Debug are for communication with the user. Using them for comments isn&#39;t as helpful.
&mdash; Chris H. (@LogicalDiagram) [February 14, 2014](https://twitter.com/LogicalDiagram/statuses/434414738660155392)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


As Chris ([@LogicalDiagram](https://twitter.com/logicaldiagram)) notes, in PowerShell, Write-Verbose and Write-Debug (in addition to Write-Warning) are great for providing additional streams of information that can provide context and logging throughout an operation. &nbsp;These are not comments. &nbsp;They can serve some of the purposes of comments. &nbsp;They differ from comments in that they provide actual runtime value

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@pcgeek86](https://twitter.com/pcgeek86) [@StevenMurawski](https://twitter.com/StevenMurawski) so I think if you pointed out that the caliber and/or type of the comments matter you&#39;d have a stronger point.
&mdash; Jason Morgan (@RJasonMorgan) [February 14, 2014](https://twitter.com/RJasonMorgan/statuses/434406183831494656)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


Regardless of the caliber of the comments, they are exceptional circumstances and should be treated as such. &nbsp;Alternatives, such as refactoring or adding a logging statement should be considered first. &nbsp;If those don't make sense or cannot add clarity, then and only then should be look to add a comment.

 
   <blockquote class="twitter-tweet" data-conversation="none" lang="en">

[@RJasonMorgan](https://twitter.com/RJasonMorgan) &gt; I think the intent behind the article was to let your code do the commenting. [@pcgeek86](https://twitter.com/pcgeek86) [@StevenMurawski](https://twitter.com/StevenMurawski)
&mdash; Sunny Chakraborty (@sunnyc7) [February 14, 2014](https://twitter.com/sunnyc7/statuses/434407824127299584)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 



Sunny ([@sunnyc7](https://twitter.com/sunnyc7)) catches my drift. &nbsp;The code should speak for itself. &nbsp;Verbose and debug logging adds context and enhances the runtime experience. &nbsp;


# Where Does That Leave Me?



I still think comments indicate a "code smell". &nbsp;In striving for cleaner code, we should seek to minimize the need for comments by writing clearer, more readable code. &nbsp; I want to thank everyone who cared enough to share their thoughts on Twitter and [via comments to the previous blog post.](http://stevenmurawski.com/powershell/2014/2/just-stop-with-the-comments-already)


<br>

