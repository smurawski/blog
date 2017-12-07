---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2014-02-01T00:00:00Z
tags: []
title: Just Stop With The Comments Already...

---

in your code.


>

the only truly good comment is the comment you found a way not to write.




  <div class="product-block" class="clear">

    

    <div class="productDetails left">
      <a href="http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882%3FSubscriptionId%3D0ENGV10E9K9QDNSJ5C82%26tag%3Dinvestipendin-20%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D0132350882" target="new" class="product-title title">Clean Code: A Handbook of Agile Software Craftsmanship</a>
      
      <div class="product-author author">By Robert C. Martin</div>
      

    </div>

  </div>



I'm working my way back through Robert Martin ([Uncle Bob](https://twitter.com/unclebobmartin))'s Clean Code and I love his thoughts on code comments. &nbsp;I'm going to highlight a few and how I think they apply to PowerShell.


## But, How Will People Know What My Code Does Without Comments?



If you have to ask that, I'm taking away your text editor.


>

**Comments Do Not Make Up for Bad Code**


One of the more common motivations for writing comments is bad code. We write a module and we know it is confusing and disorganized.




But what if my function has to do something weird because of the behavior of some other module?


>

Any comment that forces you to look in another module for the meaning of that comment has failed to communicate to you and is not worth the bits it consumes.




Then you should wrap that module with either another function or another module and use the code to express why things are being done weirdly. &nbsp;


>

It takes only a few seconds of thought to explain most of your intent in code. In many cases it’s simply a matter of creating a function that says the same thing as the comment you want to write.




To couple your implementation and require me to fully understand some other module just to get your command is a failure in your code's attempt to communicate it's intent.


## &nbsp;What's The Purpose Of Comments Then?



>

The proper use of comments is to compensate for our failure to express ourself in code. Note that I used the word failure. I meant it. Comments are always failures.




If you've read&nbsp;[my post on keeping your functions short and to the point](https://www.google.com/url?sa=t&amp;rct=j&amp;q=&amp;esrc=s&amp;source=web&amp;cd=1&amp;cad=rja&amp;ved=0CCYQFjAA&amp;url=http%3A%2F%2Fstevenmurawski.com%2Fpowershell%2F2014%2F1%2Fkeep-it-short&amp;ei=qzn-UuuAFYmGhQechoCwDg&amp;usg=AFQjCNHH4SEuYHJ716v5FR1tClQsJHlH4A&amp;bvm=bv.61190604,d.ZG4), then you should already be minimizing the the number of lines of code you have to comprehend to identify what a function does. &nbsp;More importantly, if the function is named properly, the name should tell you what is going to happen. &nbsp;Also, your variable names and the functions you call should have appropriate names as well, letting the code become self documenting.


## Comments Don't Age Well



>

Inaccurate comments are far worse than no comments at all. They delude and mislead.




Another reason that comments are poor communicators is that they tend not to be updated as the code evolves. &nbsp;This leads to stale and, more dangerously, inaccurate documentation of how the system performs. &nbsp;If it's oh-dark-thirty and you are trying to understand why a scheduled job fails and the comments are misleading, you could be wasting valuable time trying to figure out the real meaning/process.


## Yes, This Is Strict And Definitive



I'm being pretty blunt about the fact **much of the common practice sucks**. &nbsp;


I'm even condemning a bunch of code that I've written, publicly or internally at my various jobs. &nbsp;This is part of my effort to become a better scripter and a better professional. &nbsp;


If you read this and feel defensive or hurt - bring it on.. that's what the comments are for ( and twitter.. I'm [@stevenmurawski&nbsp;](https://twitter.com/stevenmurawski)). &nbsp;


**NOTE: Comment-based help in PowerShell and other comments for documentation generation are an exception to this rule.**


### **UPDATE - [Great video commentary](http://simpleprogrammer.com/2014/02/13/comment-code/) on this topic from John Sonmez ([@jsonmez](https://twitter.com/jsonmez)) from just the other day.**


