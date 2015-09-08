---
layout: post
title: "Convert Your Comments to Help"
date: 2011-04-27 11:57
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames]
---


Version 2 of PowerShell introduced [structured help](/blog/2011/04/looking-for-help) in their work, without having to write MAML help files.



### Why Not Write My Own Help Function?




One of the things I saw a good bit of in the <a href="http://bit.ly/2011sgall" target="_blank">Scripting Games</a> this year was that people wrote their own help functions.&#160; 



The usual pattern included if the script was run with a particular switch or no switch, like My-Script.ps1 –help, a little help text would be spewed out.&#160; I use that word (spewed) specifically, as the help text is often just printed to the console and does not work with Get-Help ([and all the cool things I talked about with that yesterday](/blog/2011/04/looking-for-help)..).&#160; Even if the author went through the trouble of making the output of his help function look like a PowerShell help file, you still loose the capabilities to do things like 



get-help my-command.ps1 –parameter identity</pre>

    
    With just a **less** **work**, you could turn that help function into Comment Based Help.
    

    
    I say **less work**, because instead of having to write a function and set a parameter and logic to call that help function, the you can just wrap the help in block comments and put a period in front of the keywords.
    

    
    ### What it Looks Like..
    
    
<pre language="powershell">&lt;# 
   .Synopsis 
    Do-Something makes the magic happen 
   .Example Get-ADUser | Do-Something
    Takes an ADUser and does something to that user.    
   .Parameter  Identity
    Takes an ADUser
   .Notes 
    NAME: Do-Something
    AUTHOR: Steve@UsePowerShell.com
    LASTEDIT: 04/27/2011 12:11:58 
    KEYWORDS: 
   .Link 
    Http://blog.usepowershell.com 
#&gt; </pre>

    
    Or if you really like the hash symbol
    
<pre language="powershell">#.Synopsis 
#Do-Something makes the magic happen 
#.Example Get-ADUser | Do-Something
#Takes an ADUser and does something to that user.    
#.Parameter  Identity
#Takes an ADUser
#.Notes 
#NAME: Do-Something
#AUTHOR: Steve@UsePowerShell.com
#LASTEDIT: 04/27/2011 12:11:58 
#KEYWORDS: 
#.Link 
#Http://blog.usepowershell.com </pre>

    
    ### What Can You Add?
    
    

    
    I won’t go into great detail on what the segments of help can be, as the Technet documentation does a great job, but I will highlight the different segments.
    


*   Synopsis 
*   Description 
*   Parameter (one for each parameter the function or script can take) 
*   Example (as many as your heart desires or you need to show the different use cases for the function or script) 
*   Inputs 
*   Outputs (please use this if you export a custom object and describe what is contained in it) 
*   Notes 
*   Link (to online help or resources OR other related topics.&#160; Can be used multiple times, once for each related resource) 
*   Component 
*   Role 
*   Functionality 
*   ForwardHelpTargetName (to redirect your help topic) 
*   ForwardHelpCategory 
*   RemoteHelpRunspace (used by Export-PSSession) 
*   ExternalHelp 

    
    ### Where Does It Go?
    
    

    
    Comment Based Help can be stored at the beginning of a function,
    
<pre language="powershell">function Do-Something()
{
    &lt;# 
       .Synopsis 
        Do-Something makes the magic happen 
       .Example Get-ADUser | Do-Something
        Takes an ADUser and does something to that user.    
       .Parameter  Identity
        Takes an ADUser
       .Notes 
        NAME: Do-Something
        AUTHOR: Steve@UsePowerShell.com
        LASTEDIT: 04/27/2011 12:11:58 
        KEYWORDS: 
       .Link 
        Http://blog.usepowershell.com 
    #&gt;
    param (
        [Parameter(ValueFromPipeline=$true)
        [Microsoft.ActiveDirectory.Management.ADUser]
        $Identity)
    process
    {
        #Do Something of Value here
    }

}</pre>

    
    at the end of a function,
    
<pre language="powershell">function Do-Something()
{
    param (
        [Parameter(ValueFromPipeline=$true)
        [Microsoft.ActiveDirectory.Management.ADUser]
        $Identity)
    process
    {
        #Do Something of Value here
    }
    &lt;# 
       .Synopsis 
        Do-Something makes the magic happen 
       .Example Get-ADUser | Do-Something
        Takes an ADUser and does something to that user.    
       .Parameter  Identity
        Takes an ADUser
       .Notes 
        NAME: Do-Something
        AUTHOR: Steve@UsePowerShell.com
        LASTEDIT: 04/27/2011 12:11:58 
        KEYWORDS: 
       .Link 
        Http://blog.usepowershell.com 
    #&gt;
}</pre>

    
    or before the function keyword.
    
<pre language="powershell">&lt;# 
   .Synopsis 
    Do-Something makes the magic happen 
   .Example Get-ADUser | Do-Something
    Takes an ADUser and does something to that user.    
   .Parameter  Identity
    Takes an ADUser
   .Notes 
    NAME: Do-Something
    AUTHOR: Steve@UsePowerShell.com
    LASTEDIT: 04/27/2011 12:11:58 
    KEYWORDS: 
   .Link 
    Http://blog.usepowershell.com 
#&gt;
function Do-Something()
{
    param (
        [Parameter(ValueFromPipeline=$true)
        [Microsoft.ActiveDirectory.Management.ADUser]
        $Identity)
    process
    {
        #Do Something of Value here
    }
}



**NOTE: In a script, you can only use the before the body of the script or after the body of the script, since the script is self contained in one file.**



&#160;



### And if you order right now..




And if that’s not easy enough, there are several functions that have been written to help you create your comment based help.



*   <a href="http://jdhitsolutions.com/blog/2011/03/new-comment-help/" target="_blank">From Jeff Hicks.</a> 
*   <a href="http://blogs.technet.com/b/heyscriptingguy/archive/2010/09/11/automatically-add-comment-based-help-to-your-powershell-scripts.aspx" target="_blank">From Ed Wilson</a> 
