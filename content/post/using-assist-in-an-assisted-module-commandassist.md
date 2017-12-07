---
author: steven.murawski@gmail.com
categories:
- CommandAssist
- Modules
- Scripts
comments: true
date: 2011-08-01T00:00:00Z
tags: []
title: Using -Assist in an Assisted Module - CommandAssist

---

This is the the meat of CommandAssist module, providing a contextual assistant to supplying the right parameters to a command.



### Getting to the Meat of It




In the last [post](/blog/2011/08/creating-an-assisted-module-with-commandassist), we created a proxy module for ActiveDirectory module, so what does that really get for us?



Starting off in a new shell, I've imported the AssistedAD module and I'm going to try out the -Assist parameter for Get-ADUser



Import-Module ADAssistGet-ADUser -Assist



Initially, I was prompted for a Filter parameter, as that parameter is marked mandatory in the default parameter set. I supplied a &quot;*&quot; and moved on. You'll notice that I did not have to load the ActiveDirectory module, the CommandAssist module or the <a href="http://showui.codeplex.com/" target="_blank">ShowUI</a> module, as the proxy module handles all that in the background for you.



What I then got was:



&#160;



<a href="http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfb92/1313229027000/?format=original">![image](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfb95/1313229028000/?format=original "image")</a>



You'll notice that there are three tabs, Filter, LdapFilter, and Identity.&#160; Each tab represents a parameter set and shows the associated parameters for that set.&#160; 



You will only be able to pass parameters for one parameter set, which can alleviate the common error of 



>


Parameter set cannot be resolved using the specified named parameters






This is a key benefit of CommandAssist versus other tab completion options.



As you add parameters, the command line at the top will change to reflect your changes to the parameters.



<a href="http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfb98/1313229029000/?format=original">![image](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfb9b/1313229031000/?format=original "image")</a>



Parameters that take a specified enumeration (like AuthType in this case) will have a dropdown box with the available options.



If I change parameter sets, the command line changes to reflect my new parameter selections.



<a href="http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfb9e/1313229032000/?format=original">![image](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf4c2e4b0c945709cfba1/1313229033000/?format=original "image")</a>



However, your previous selections on an alternate tab have not changed, so you can return to that tab and continue editing if you determine that is the appropriate parameter set to use.



&#160;



### What other information would you like to see here?




Please post a comment on the blog or a message on Twitter if any additional information or features you think might be useful for this!&#160; I'd love to hear how you think this might provide value in your usage or your environment.

