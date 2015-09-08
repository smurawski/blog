---
layout: post
title: "Looking for Help?"
date: 2011-04-26 09:16
author: steven.murawski@gmail.com
comments: true
categories: [PowerShell, PowerShell Version 2, Scripting Games, Tip]
tags: [2011ScriptingGames]
---


PowerShell contains a vast array of content in it's help files.&#160; The help files are exposed by the <a href="http://go.microsoft.com/fwlink/?LinkID=113316" target="_blank">Get-Help cmdlet</a>.&#160; So, what's really under the covers in the help system?



## Finding Help by Keyword




PowerShell has a growing surface of things that it can cover.&#160; WMI, COM, .NET, Exchange, Active Directory, SharePoint, and on and on.&#160; How are we ever supposed to find the commands and information we need to use PowerShell effectively.&#160; Get-Help gives us some options. 



First off, Get-Help let's us search all the help it has access to (see below on where the help comes from) by a keyword.&#160; 



Let's start with an example.&#160; I'm going to compete in the Beginner category of the <a href="http://bit.ly/2011sgall" target="_blank">Scripting Games</a> for Event #1.&#160; That event wants me to find out if an program being run is a private build or not.&#160; Since the program is running, we need to deal with processes.&#160; But what can PowerShell do with processes?



PS C:\Users\smurawski&gt; Get-Help process
Name                              Category  Synopsis
----                              --------  --------
Get-Process                       Cmdlet    Gets the processes that are running on the local computer or a remote co...
Stop-Process                      Cmdlet    Stops one or more running processes.
Wait-Process                      Cmdlet    Waits for the processes to be stopped before accepting more input.
Debug-Process                     Cmdlet    Debugs one or more processes running on the local computer.
Start-Process                     Cmdlet    Starts one or more processes on the local computer.</pre>

    
    So, that gives me a list of commands that refer to process in their help.&#160; I've got a starting place now!
    

    
    ## Finding Help by Cmdlet (or Script or Function)
    
    

    
    Now that I've narrowed down my search to a handful of commands, one really jumps out at me - Get-Process.
    
<pre language="powershell">PS C:\Users\smurawski&gt; Get-Help Get-Process

NAME
    Get-Process

SYNOPSIS
    Gets the processes that are running on the local computer or a remote computer.

SYNTAX
    Get-Process [[-Name] <string  []>] [-ComputerName <string  []>] [-FileVersionInfo] [-Module] [<commonparameters>]

    Get-Process -Id <int32  []> [-ComputerName <string  []>] [-FileVersionInfo] [-Module] [<commonparameters>]

    Get-Process -InputObject 
<process  []> [-ComputerName <string  []>] [-FileVersionInfo] [-Module] [<commonparameters>]

DESCRIPTION
    The Get-Process cmdlet gets the processes on a local or remote computer.

    Without parameters, Get-Process gets all of the processes on the local computer. You can also specify a particular process by process name or process ID (PID) or pass a process object through the pipeline to Get-Process.

    By default, Get-Process returns a process object that has detailed information about the process and supports methods that let you start and stop the process. You can also use the parameters of Get-Process to get file version information for the program that runs in the process and to get the modules that the process loaded.

RELATED LINKS
    Online version: http://go.microsoft.com/fwlink/?LinkID=113324
    Get-Process
    Start-Process
    Stop-Process
    Wait-Process
    Debug-Process

REMARKS
    To see the examples, type: &quot;get-help Get-Process -examples&quot;.
    For more information, type: &quot;get-help Get-Process -detailed&quot;.
    For technical information, type: &quot;get-help Get-Process -full&quot;.</pre>

    
    &#160;
    

    
    Checking out the help provided, I can see the various ways that I can call Get-Process (also known as the Parameter Sets).&#160; I have a synopsis and description for the command, related commands, and finally some remarks.
    

    
    ### Help Levels
    
    

    
    The remarks offer us several options to how we can access Get-Help to provide additional information.
    

    
    The first option is Detailed.&#160; Detailed help provides addition descriptions of each of the parameters, as well as examples of usage.&#160; For example:
    
<pre language="powershell">PARAMETERS
    -ComputerName <string  []>
        Gets the processes running on the specified computers. The default is the local computer.

        Type the NetBIOS name, an IP address, or a fully qualified domain name of one or more computers. To specify the local computer, type the computer name, a dot (.), or &quot;localhost&quot;.

        This parameter does not rely on Windows PowerShell remoting. You can use the ComputerName parameter of Get-Process even if your computer is not configured to run remote commands.</pre>

    
    and
    
<pre language="powershell">-------------------------- EXAMPLE 1 --------------------------

C:\PS&gt;Get-Process

Description
-----------
This command gets a list of all of the running processes running on the local computer. For a definition of each column, see the &quot;Additional Notes&quot; section of the Help topic for Get-Help.</pre>

    
    The second option is Full.&#160; Full offers a further details on parameters like whether or not the value is required or can be filled with pipeline input.
    
<pre language="powershell">PARAMETERS
    -ComputerName <string  []>
        Gets the processes running on the specified computers. The default is the local computer.

        Type the NetBIOS name, an IP address, or a fully qualified domain name of one or more computers. To specify the local computer, type the computer name, a dot (.), or &quot;localhost&quot;.

        This parameter does not rely on Windows PowerShell remoting. You can use the ComputerName parameter of Get-Process even if your computer is not configured to run remote commands.

        Required?                    false
        Position?                    named
        Default value
        Accept pipeline input?       true (ByPropertyName)
        Accept wildcard characters?  false</pre>

    
    ### Other Options
    
    

    
    Get-Help also allows you to do other interesting things like get a refresher on a particular parameter.
    
<pre language="powershell">PS C:\Users\smurawski&gt; get-help Get-Process -Parameter FileVersionInfo

-FileVersionInfo [<switchparameter>]
    Gets the file version information for the program that runs in the process.

    On Windows Vista and later versions of Windows, you must open Windows PowerShell with the &quot;Run as administrator&quot; option to use this parameter on processes that you do not own.

    Using this parameter is equivalent to getting the MainModule.FileVersionInfo property of each process object. When you use this parameter, Get-Process returns a FileVersionInfo object (System.Diagnostics.FileVersionInfo), not a process object. So, you cannot pipe the output of the command to a cmdlet that expects a process object, such as Stop-Process.

    Required?                    false
    Position?                    named
    Default value
    Accept pipeline input?       false
    Accept wildcard characters?  false</pre>

    
    Sometimes you just need a quick example to get you going.
    
<pre language="powershell">NAME
    Get-Process

SYNOPSIS
    Gets the processes that are running on the local computer or a remote computer.

    -------------------------- EXAMPLE 1 --------------------------

    C:\PS&gt;Get-Process

    Description
    -----------
    This command gets a list of all of the running processes running on the local computer. For a definition of each column, see the &quot;Additional Notes&quot; section of the Help topic for Get-Help.
    ...



NOTE:There are more examples for this command, I've just truncated the output to save space.



## Finding Help from About Topics




Get-Help is not limited to getting help for commands.&#160; PowerShell comes with a rich set of documentation, all available from the shell.&#160; Get-Help about_* will provide you a list of all the great help topics available.



If you've always had a burning desire to learn more about how PowerShell deals with quoting, you could use Get-Help about_Quoting_Rules.



## Going Online for the Latest Updates




The last tip/trick that Get-Help leaves us is the -Online parameter.&#160; If a help file is configured with a link, this parameter will open your default browser and go to that link.&#160; The effect is that you can always find the latest version of the online documentation for commands configured that way.&#160; All of the core commands include online help, and anyone who publishes scripts, functions, or modules can host their own online help as well.



## Where does the Help Come From?




The help files are stored in a couple of ways.&#160; First off, there are a series of XML files (<a href="http://en.wikipedia.org/wiki/Microsoft_Assistance_Markup_Language" target="_blank">using MAML - Microsoft Assistance Markup Language</a>) that contain the help under a localized directory in $pshome (an automatic variable that points to where PowerShell is installed).&#160; 



Snapin and module authors can include MAML files to provide help to users of those items (there is a tool to help create the MAML files - the <a href="http://blogs.msdn.com/b/powershell/archive/2011/02/24/cmdlet-help-editor-v2-0-with-module-support.aspx" target="_blank">Cmdlet Help Editor</a>).&#160; 



Finally, scripters can create <a href="http://go.microsoft.com/fwlink/?LinkID=144309" target="_blank">Comment Based Help</a> for their scripts and functions.&#160; The cool thing about that is that your scripts and functions can behave exactly like the built in commands in providing help on usage, parameters, and examples.&#160; If you are really ambitious, you can also create online documentation and link to that.

