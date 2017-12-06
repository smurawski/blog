---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-10-01T00:00:00Z
tags: []
title: The Cloud is Coming...

---

Interesting twitter conversation between Jeffrey Snover and several others on Twitter about the recent Cloud Platform System (CPS) announced in conjunction with Dell. &nbsp;


## TL;DR;

<p dir="ltr">Companies are trying to implement private clouds (and failing). &nbsp;
<p dir="ltr">**Isn't a private cloud just your own data center with some virtualization tossed in?** &nbsp;Nope! &nbsp;There is a level of automation and standardization of offerings to the consumers of your private cloud resources. &nbsp;Networking, storage, and compute all become part of the offering that is surfaced to internal consumers.
<p dir="ltr">**Does the Microsoft CPS really get me Azure locally?&nbsp;**Not really. &nbsp;Close, though. &nbsp;Much of the experience is similar, but Azure has other service offerings and the portal gets updated more frequently than the Azure Pack portal.
<p dir="ltr">**Does the Microsoft CPS really get me a private cloud?&nbsp;**Again, not quite, but close. &nbsp;All the basic tools are there, as well as the hardware elements. &nbsp;You still have to have operations folks who can deliver and operate the service and tools provided. &nbsp;System Center isn't a "fire and forget" installation.
<p dir="ltr">**Does private cloud mean I can fire my ops folks? &nbsp;**Just the ones that can't automate their environment. &nbsp;CPS and most private cloud efforts don't even scratch the surface of configuration management for the guest machines. &nbsp;
<p dir="ltr">**$MyCompany won't go down the private cloud route, so I don't have to care. &nbsp;**Until they do.. then will your skillset be up to the task? &nbsp;As Adam states below, there is a comfort factor in doing things the old way, but how many of your organizations still barter for livestock? &nbsp;In business things change...


## Read More



Snippets of the &nbsp;thread.. They start with the first one below and then diverge as the conversation travels

 
   <blockquote class="twitter-tweet">

“Almost 80 per cent of private cloud projects today are failing". Good article on CPS by [@marypcbuk](https://twitter.com/marypcbuk) [http://t.co/gccSyskj7A](http://t.co/gccSyskj7A)
— jsnover (@jsnover) [October 21, 2014](https://twitter.com/jsnover/status/524551336130404352)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@jsnover](https://twitter.com/jsnover) [@marypcbuk](https://twitter.com/marypcbuk) When Microsoft caves and provides the "cloud" on-prem shouldn't this be concerning to people beating the cloud drum?
— Adam Bertram (@adbertram) [October 21, 2014](https://twitter.com/adbertram/status/524552700080685056)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@hitchysg](https://twitter.com/hitchysg) [@adbertram](https://twitter.com/adbertram) [@jsnover](https://twitter.com/jsnover) it's not *the* cloud; it's *a* cloud. A private cloud
— Scary Branscombe (@marypcbuk) [October 21, 2014](https://twitter.com/marypcbuk/status/524563012766937089)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@marypcbuk](https://twitter.com/marypcbuk) [@hitchysg](https://twitter.com/hitchysg) [@jsnover](https://twitter.com/jsnover) What's the difference between a private cloud and something like Cisco UCS, for example?
— Adam Bertram (@adbertram) [October 21, 2014](https://twitter.com/adbertram/status/524563175187173376)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@adbertram](https://twitter.com/adbertram) [@hitchysg](https://twitter.com/hitchysg) [@jsnover](https://twitter.com/jsnover) UCS is s'thing you can use to build a private cloud; SW-defined networking &amp; rack. Not preconfigured like CPS
— Scary Branscombe (@marypcbuk) [October 21, 2014](https://twitter.com/marypcbuk/status/524563960608342016)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

. [@marypcbuk](https://twitter.com/marypcbuk) [@hitchysg](https://twitter.com/hitchysg) [@adbertram](https://twitter.com/adbertram) We think of this as "giving you the cloud on your terms" Some want public, some hosted, some private
— jsnover (@jsnover) [October 21, 2014](https://twitter.com/jsnover/status/524564063544565761)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@adbertram](https://twitter.com/adbertram) [@marypcbuk](https://twitter.com/marypcbuk) [@hitchysg](https://twitter.com/hitchysg) [@jsnover](https://twitter.com/jsnover) Building blocks of software defined data centers. UCS needs an additional managment layer.
— Simon Bisson (@sbisson) [October 21, 2014](https://twitter.com/sbisson/status/524564153982537728)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@sbisson](https://twitter.com/sbisson) [@adbertram](https://twitter.com/adbertram) [@hitchysg](https://twitter.com/hitchysg) [@jsnover](https://twitter.com/jsnover) so you'd put PowerShell/DSC or SCVMM or other layer in to turn your pieces into actual cloud
— Scary Branscombe (@marypcbuk) [October 21, 2014](https://twitter.com/marypcbuk/status/524564297419337729)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@jsnover](https://twitter.com/jsnover) [@hitchysg](https://twitter.com/hitchysg) [@adbertram](https://twitter.com/adbertram) if you're going right back to private cloud versus data centre; it's level of standardisation, automation
— Scary Branscombe (@marypcbuk) [October 21, 2014](https://twitter.com/marypcbuk/status/524564444559720448)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@jsnover](https://twitter.com/jsnover) [@hitchysg](https://twitter.com/hitchysg) [@adbertram](https://twitter.com/adbertram) obviously, only public cloud gives you scalout w'out ever buying hardware, but some co's need local data gov
— Scary Branscombe (@marypcbuk) [October 21, 2014](https://twitter.com/marypcbuk/status/524564919220715520)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@jsnover](https://twitter.com/jsnover) [@marypcbuk](https://twitter.com/marypcbuk) That makes sense.  UCS would just be one part of the entire solution. I think I understand a little better now.
— Adam Bertram (@adbertram) [October 21, 2014](https://twitter.com/adbertram/status/524564870646464512)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@jsnover](https://twitter.com/jsnover) [@hitchysg](https://twitter.com/hitchysg) [@adbertram](https://twitter.com/adbertram) makes no sense for anyone outside Azure to use the tools Ms runs Azure with day to day
— Scary Branscombe (@marypcbuk) [October 21, 2014](https://twitter.com/marypcbuk/status/524566595839229952)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

[@marypcbuk](https://twitter.com/marypcbuk) [@jsnover](https://twitter.com/jsnover) [@hitchysg](https://twitter.com/hitchysg) I tend to agree.  It's still just a comfort thing IMO. Businesses are still reluctant to offload resources.
— Adam Bertram (@adbertram) [October 21, 2014](https://twitter.com/adbertram/status/524566804514230272)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 

 
   <blockquote class="twitter-tweet">

. [@adbertram](https://twitter.com/adbertram) [@marypcbuk](https://twitter.com/marypcbuk) [@hitchysg](https://twitter.com/hitchysg) People used to put money under their mattress to keep it safe. Now they use a bank.  [#ThingsChange](https://twitter.com/hashtag/ThingsChange?src=hash)
— jsnover (@jsnover) [October 21, 2014](https://twitter.com/jsnover/status/524571530789781504)


<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
 


<br>


<br>

