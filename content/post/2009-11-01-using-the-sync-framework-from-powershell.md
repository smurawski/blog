---
author: steven.murawski@gmail.com
categories:
- -NET Framework
- PowerShell
- PowerShell Version 2
- Scripts
- Sync Framework
comments: true
date: 2009-11-01T00:00:00Z
tags: []
title: Using the Sync Framework from PowerShell

---

I’ve been exploring the <a href="http://msdn.microsoft.com/en-us/sync/default.aspx" target="_blank">Sync Framework</a> for use in a couple of projects I have going and <a href="http://msdn.microsoft.com/en-us/library/ms714418(VS.85).aspx" target="_blank">PowerShell</a> is my preferred exploratory environment.



It was a bit of fun, since I got to work with eventing for the first time in V2.



First, I downloaded the <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=89adbb1e-53ff-41b5-ba17-8e43a2e66254&amp;displaylang=en" target="_blank">Sync Framework Software Development Kit</a>.  That provided me with the Sync Framework runtime as well as some documentation.



The easiest way for me to get started was to take one <a href="http://msdn.microsoft.com/en-us/library/ee617386(SQL.105).aspx" target="_blank">of the samples</a> and convert that to PowerShell.



I’m going to walk along the MSDN Sample and provide the equivalent PowerShell, as well as any changes I made to make it feel more PowerShell-y.



### Setting Synchronization Options




We are working with the File Sync Provider First up is setting the <a href="http://msdn.microsoft.com/en-us/library/microsoft.synchronization.files.filesyncoptions(SQL.105).aspx" target="_blank">FileSyncOptions</a>.  FileSyncOptions are an enumeration (a limited list defined in code that maps to certain values) whose values are controlled by setting the appropriate bits to indicate the presence or absence of a flag.  <a href="http://twitter.com/meson3902" target="_blank">Mark Schill</a> has <a href="http://www.cmschill.net/stringtheory/2009/05/02/bitwise-operators/" target="_blank">a great post about how to set bitwise operations</a>.



<span style="color: #ff4500">$options </span><span style="color: #a9a9a9">= </span><span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">ExplicitDetectChanges</span><span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">-bor</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">RecycleDeletedFiles</span>
<span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">-bor</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">RecyclePreviousFileOnUpdates</span>
<span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">-bor</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">RecycleConflictLoserFiles</span></pre>

    
    ### Specifying a Static Filter
    
    

    
    With the <a href="http://msdn.microsoft.com/en-us/library/bb902860(SQL.105).aspx" target="_blank">File System provider,</a> we can provide filters to include or exclude files and directories.
    

    
    $FileNameFilter and $SubdirectoryNameFilter are parameters that take strings or string arrays.
    
<pre class="PowerShellColorizedScript"><span style="color: #ff4500">$filter</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span> <span style="color: #8a2be2">Microsoft.Synchronization.Files.FileSyncScopeFilter</span>
<span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$FileNameFilter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">count</span> <span style="color: #a9a9a9">-gt</span> <span style="color: #800080">0</span><span style="color: #000000">)</span>
<span style="color: #000000">{</span>
   <span style="color: #ff4500">$FileNameFilter</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">ForEach-Object</span> <span style="color: #000000">{</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">FileNameExcludes</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Add</span><span style="color: #000000">(</span><span style="color: #ff4500">$_</span><span style="color: #000000">)</span> <span style="color: #000000">}</span>
<span style="color: #000000">}</span>
<span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$SubdirectoryNameFilter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">count</span> <span style="color: #a9a9a9">-gt</span> <span style="color: #800080">0</span><span style="color: #000000">)</span>
<span style="color: #000000">{</span>
   <span style="color: #ff4500">$SubdirectoryNameFilter</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">ForEach-Object</span> <span style="color: #000000">{</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SubdirectoryExcludes</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Add</span><span style="color: #000000">(</span><span style="color: #ff4500">$_</span><span style="color: #000000">)</span> <span style="color: #000000">}</span>
<span style="color: #000000">}</span></pre>

    
    ### Performing Change Detection
    
    

    
    After configuring the filter, we examine the folders and files located at the paths specified.  If there has not been any previous synchronization, a metadata file will be created in each location to track any changes, updates, and deletes for later synchronization.
    
<pre class="PowerShellColorizedScript"><span style="color: #00008b">function</span> <span style="color: #8a2be2">Get-FileSystemChange</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
<span style="color: #000000">{</span>
    <span style="color: #00008b">param</span> <span style="color: #000000">(</span><span style="color: #ff4500">$path</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$options</span><span style="color: #000000">)</span>
    <span style="color: #00008b">try</span>
    <span style="color: #000000">{</span>
        <span style="color: #ff4500">$provider</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">new-object</span> <span style="color: #8a2be2">Microsoft.Synchronization.Files.FileSyncProvider</span> <span style="color: #000080">-ArgumentList</span> <span style="color: #ff4500">$path</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$options</span>
        <span style="color: #ff4500">$provider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DetectChanges</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
    <span style="color: #000000">}</span>
    <span style="color: #00008b">finally</span>
    <span style="color: #000000">{</span>
        <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$provider</span> <span style="color: #a9a9a9">-ne</span> <span style="color: #ff4500">$null</span><span style="color: #000000">)</span>
        <span style="color: #000000">{</span>
            <span style="color: #ff4500">$provider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Dispose</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
        <span style="color: #000000">}</span>
    <span style="color: #000000">}</span>
<span style="color: #000000">}</span></pre>

    
     
    
<pre class="PowerShellColorizedScript"><span style="color: #0000ff">Get-FileSystemChange</span> <span style="color: #ff4500">$SourcePath</span> <span style="color: #ff4500">$filter</span> <span style="color: #ff4500">$options</span>
<span style="color: #0000ff">Get-FileSystemChange</span> <span style="color: #ff4500">$DestinationPath</span> <span style="color: #ff4500">$filter</span> <span style="color: #ff4500">$options</span></pre>

    
     
    

    
    ### Handling Conflicts
    
    

    
    Conflict resolution in the Sync Framework happens at the at the event level.  An event is merely something that happens that can trigger other actions.  Using <a href="http://technet.microsoft.com/en-us/library/dd347672.aspx" target="_blank">Register-ObjectEvent</a>, we can associate one or more scriptblocks with an event.
    

    
    First, I defined scriptblocks to handle the conflicts.  There is an enumeration, the <a href="http://msdn.microsoft.com/en-us/library/microsoft.synchronization.conflictresolutionaction(SQL.105).aspx" target="_blank">ConflictResolutionAction</a> enumeration, that provides some options for dealing with conflicts.  For this example, we are going to pick the source object as the winner for any conflicts.
    

    
    You will also notice another type of conflict defined, and that is a Constraint conflict.  That can occur when an object of the same name is added on both sides in between synchronizations.  The resolution options for these conflicts can be found in the <a href="http://msdn.microsoft.com/en-us/library/microsoft.synchronization.constraintconflictresolutionaction(SQL.105).aspx" target="_blank">ConstraintConflictResolutionAction</a> enumeration.
    
<pre class="PowerShellColorizedScript"><span style="color: #ff4500">$ItemConflictAction</span> <span style="color: #a9a9a9">=</span>   <span style="color: #000000">{</span>
    <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SetResolutionAction</span><span style="color: #000000">(</span><span style="color: #008080">[Microsoft.Synchronization.ConflictResolutionAction]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">SourceWins</span><span style="color: #000000">)</span>
    <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Conflicted</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationChange</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ItemId</span>
<span style="color: #000000">}</span>
<span style="color: #ff4500">$ItemConstraintAction</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">{</span>
     <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SetResolutionAction</span><span style="color: #000000">(</span><span style="color: #008080">[Microsoft.Synchronization.ConstraintConflictResolutionAction]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">SourceWins</span><span style="color: #000000">)</span>
     <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Constrained</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationChange</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ItemId</span>
<span style="color: #000000">}</span>            

<span style="color: #006400"># Configure the events for conflicts or constraints for the source and destination providers</span>
<span style="color: #ff4500">$destinationCallbacks</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$destinationProvider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationCallbacks</span>
<span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$destinationCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConflicting</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConflictAction</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Out-Null</span>
<span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$destinationCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConstraint</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConstraintAction</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Out-Null</span>            

<span style="color: #ff4500">$sourceCallbacks</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$SourceProvider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationCallbacks</span>
<span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$sourceCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConflicting</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConflictAction</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Out-Null</span>
<span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$sourceCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConstraint</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConstraintAction</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Out-Null</span></pre>

    
    We also see for the first time in the script blocks a variable called $event.  This is an automatic variable exposed by the event and provides us information that we can use in our action.
    

    
    Finally, I’m updating a variable in the global scope.  There probably is a better way to handle this, but scriptblocks executed in response to events only have access to the global scope and any of the automatic variable exposed to it.  Therefore, I use a variable in the global scope to gather my reporting information.
    

    
    ### Synchronizing Two Replicas
    
    

    
    To start to synchronize the two sides, first we set up the synchronization via a <a href="http://msdn.microsoft.com/en-us/library/microsoft.synchronization.syncorchestrator.aspx" target="_blank">SyncOrchestrator</a> and assign it the local and remote providers, as well as defining the direction of the synchronization.  In this example (sticking with the format from MSDN, we will do an Upload, which is in the <a href="http://msdn.microsoft.com/en-us/library/microsoft.synchronization.syncdirectionorder.aspx" target="_blank">SyncDirectionOrder enumeration</a> (other options are Download, DownloadAndUpload, and UploadAndDownload). 
    
<pre class="PowerShellColorizedScript"><span style="color: #006400"># Create the agent that will perform the file sync</span>
<span style="color: #ff4500">$agent</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span>  <span style="color: #8a2be2">Microsoft.Synchronization.SyncOrchestrator</span>
<span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">LocalProvider</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$sourceProvider</span>
<span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">RemoteProvider</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$destinationProvider</span>            

<span style="color: #006400"># Upload changes from the source to the destination.</span>
<span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Direction</span> <span style="color: #a9a9a9">=</span> <span style="color: #008080">[Microsoft.Synchronization.SyncDirectionOrder]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">Upload</span>            

<span style="color: #0000ff">Write-Host</span> <span style="color: #8b0000">"Synchronizing changes from $($sourceProvider.RootDirectoryPath) to replica: $($destinationProvider.RootDirectoryPath)"</span>
<span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Synchronize</span><span style="color: #000000">(</span><span style="color: #000000">)</span><span style="color: #000000">;</span></pre>

    
    To achieve two way synchronization, we will do the upload twice, reversing the order of the providers.
    
<pre class="PowerShellColorizedScript"><span style="color: #0000ff">Invoke-OneWayFileSync</span> <span style="color: #000080">-SourcePath</span> <span style="color: #ff4500">$SourcePath</span> <span style="color: #000080">-DestinationPath</span> <span style="color: #ff4500">$DestinationPath</span> <span style="color: #000080">-Filter</span> <span style="color: #ff4500">$null</span> <span style="color: #000080">-Options</span> <span style="color: #ff4500">$options</span>
<span style="color: #0000ff">Invoke-OneWayFileSync</span> <span style="color: #000080">-SourcePath</span> <span style="color: #ff4500">$DestinationPath</span> <span style="color: #000080">-DestinationPath</span> <span style="color: #ff4500">$SourcePath</span> <span style="color: #000080">-Filter</span> <span style="color: #ff4500">$null</span> <span style="color: #000080">-Options</span> <span style="color: #ff4500">$options</span></pre>

    
    ### Full Example
    
    

    
    I modified the example to write out a custom object (and the logging is in the variable in the global scope as noted in the Handling Conflicts section) with the results of the synchronization (rather than logging it to the console).
    

    
    In all, my translation is pretty similar to the example code, but there are some differences. 
    
<pre class="PowerShellColorizedScript"><span style="color: #006400"># Requires -Version 2</span>
<span style="color: #006400"># Also depends on having the Microsoft Sync Framework 2.0 SDK or Runtime</span>
<span style="color: #006400"># --SDK--</span>
<span style="color: #006400"># http://www.microsoft.com/downloads/details.aspx?FamilyID=89adbb1e-53ff-41b5-ba17-8e43a2e66254&amp;displaylang=en</span>
<span style="color: #006400"># --Runtime--</span>
<span style="color: #006400"># http://www.microsoft.com/downloads/details.aspx?FamilyId=109DB36E-CDD0-4514-9FB5-B77D9CEA37F6&amp;displaylang=en</span>
<span style="color: #006400">#</span>
<span style="color: #006400">#</span>            

<span style="color: #a9a9a9">[</span><span style="color: #add8e6">CmdletBinding</span><span style="color: #000000">(</span><span style="color: #000000">SupportsShouldProcess</span><span style="color: #a9a9a9">=</span><span style="color: #ff4500">$true</span><span style="color: #000000">)</span><span style="color: #a9a9a9">]</span>
<span style="color: #00008b">param</span> <span style="color: #000000">(</span>
    <span style="color: #a9a9a9">[</span><span style="color: #add8e6">Parameter</span><span style="color: #000000">(</span><span style="color: #000000">Position</span><span style="color: #a9a9a9">=</span><span style="color: #800080">1</span><span style="color: #a9a9a9">,</span> <span style="color: #000000">Mandatory</span><span style="color: #a9a9a9">=</span><span style="color: #ff4500">$true</span><span style="color: #a9a9a9">,</span> <span style="color: #000000">ValueFromPipelineByPropertyName</span><span style="color: #a9a9a9">=</span><span style="color: #ff4500">$true</span><span style="color: #000000">)</span><span style="color: #a9a9a9">]</span>
    <span style="color: #a9a9a9">[</span><span style="color: #add8e6">Alias</span><span style="color: #000000">(</span><span style="color: #8b0000">'FullName'</span><span style="color: #a9a9a9">,</span> <span style="color: #8b0000">'Path'</span><span style="color: #000000">)</span><span style="color: #a9a9a9">]</span>
    <span style="color: #008080">[string]</span><span style="color: #ff4500">$SourcePath</span>
    <span style="color: #a9a9a9">,</span> <span style="color: #a9a9a9">[</span><span style="color: #add8e6">Parameter</span><span style="color: #000000">(</span><span style="color: #000000">Position</span><span style="color: #a9a9a9">=</span><span style="color: #800080">2</span><span style="color: #a9a9a9">,</span> <span style="color: #000000">Mandatory</span><span style="color: #a9a9a9">=</span><span style="color: #ff4500">$true</span><span style="color: #000000">)</span><span style="color: #a9a9a9">]</span>
    <span style="color: #008080">[string]</span><span style="color: #ff4500">$DestinationPath</span>
    <span style="color: #a9a9a9">,</span> <span style="color: #a9a9a9">[</span><span style="color: #add8e6">Parameter</span><span style="color: #000000">(</span><span style="color: #000000">Position</span><span style="color: #a9a9a9">=</span><span style="color: #800080">3</span><span style="color: #000000">)</span><span style="color: #a9a9a9">]</span>
    <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$FileNameFilter</span>
    <span style="color: #a9a9a9">,</span> <span style="color: #a9a9a9">[</span><span style="color: #add8e6">Parameter</span><span style="color: #000000">(</span><span style="color: #000000">Position</span><span style="color: #a9a9a9">=</span><span style="color: #800080">4</span><span style="color: #000000">)</span><span style="color: #a9a9a9">]</span>
    <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$SubdirectoryNameFilter</span>
<span style="color: #000000">)</span>
<span style="color: #006400">&lt;#
    .Synopsis
        Synchronizes to directory trees
    .Description
        Examines two directory structures (SourcePath and DestinationPath) and uses the Microsoft Sync Framework
        File System Provider to synchronize them.
    .Example
        An example of using the command
#&gt;</span>
<span style="color: #00008b">begin</span>
<span style="color: #000000">{</span>
    <span style="color: #008080">[reflection.assembly]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">LoadWithPartialName</span><span style="color: #000000">(</span><span style="color: #8b0000">'Microsoft.Synchronization'</span><span style="color: #000000">)</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Out-Null</span>
    <span style="color: #008080">[reflection.assembly]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">LoadWithPartialName</span><span style="color: #000000">(</span><span style="color: #8b0000">'Microsoft.Synchronization.Files'</span><span style="color: #000000">)</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">Out-Null</span>            

    <span style="color: #00008b">function</span> <span style="color: #8a2be2">Get-FileSystemChange</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
    <span style="color: #000000">{</span>
        <span style="color: #00008b">param</span> <span style="color: #000000">(</span><span style="color: #ff4500">$path</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$options</span><span style="color: #000000">)</span>
        <span style="color: #00008b">try</span>
        <span style="color: #000000">{</span>
            <span style="color: #ff4500">$provider</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">new-object</span> <span style="color: #8a2be2">Microsoft.Synchronization.Files.FileSyncProvider</span> <span style="color: #000080">-ArgumentList</span> <span style="color: #ff4500">$path</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$options</span>
            <span style="color: #ff4500">$provider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DetectChanges</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
        <span style="color: #000000">}</span>
        <span style="color: #00008b">finally</span>
        <span style="color: #000000">{</span>
            <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$provider</span> <span style="color: #a9a9a9">-ne</span> <span style="color: #ff4500">$null</span><span style="color: #000000">)</span>
            <span style="color: #000000">{</span>
                <span style="color: #ff4500">$provider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Dispose</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
            <span style="color: #000000">}</span>
        <span style="color: #000000">}</span>
    <span style="color: #000000">}</span>            

    <span style="color: #00008b">function</span> <span style="color: #8a2be2">Invoke-OneWayFileSync</span><span style="color: #000000">(</span><span style="color: #000000">)</span>
    <span style="color: #000000">{</span>
        <span style="color: #00008b">param</span> <span style="color: #000000">(</span><span style="color: #ff4500">$SourcePath</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$DestinationPath</span><span style="color: #a9a9a9">,</span>
                    <span style="color: #ff4500">$Filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$Options</span><span style="color: #000000">)</span>
        <span style="color: #ff4500">$ApplyChangeJobs</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">@(</span><span style="color: #000000">)</span>
        <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">@(</span><span style="color: #000000">)</span>
        <span style="color: #00008b">try</span>
        <span style="color: #000000">{</span>
            <span style="color: #006400"># Scriptblocks to handle the events raised during synchronization</span>
            <span style="color: #ff4500">$AppliedChangeAction</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">{</span>
                <span style="color: #ff4500">$argument</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span>
                <span style="color: #00008b">switch</span> <span style="color: #000000">(</span><span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ChangeType</span><span style="color: #000000">)</span>
                <span style="color: #000000">{</span>
                    <span style="color: #000000">{</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ChangeType</span> <span style="color: #a9a9a9">-eq</span> <span style="color: #008080">[Microsoft.Synchronization.Files.ChangeType]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">Create</span> <span style="color: #000000">}</span> <span style="color: #000000">{</span><span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Created</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">NewFilePath</span><span style="color: #000000">}</span>
                    <span style="color: #000000">{</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ChangeType</span> <span style="color: #a9a9a9">-eq</span> <span style="color: #008080">[Microsoft.Synchronization.Files.ChangeType]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">Delete</span> <span style="color: #000000">}</span> <span style="color: #000000">{</span><span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Deleted</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">OldFilePath</span><span style="color: #000000">}</span>
                    <span style="color: #000000">{</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ChangeType</span> <span style="color: #a9a9a9">-eq</span> <span style="color: #008080">[Microsoft.Synchronization.Files.ChangeType]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">Update</span> <span style="color: #000000">}</span> <span style="color: #000000">{</span><span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Updated</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">OldFilePath</span><span style="color: #000000">}</span>
                    <span style="color: #000000">{</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ChangeType</span> <span style="color: #a9a9a9">-eq</span> <span style="color: #008080">[Microsoft.Synchronization.Files.ChangeType]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">Rename</span> <span style="color: #000000">}</span> <span style="color: #000000">{</span><span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Renamed</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$argument</span><span style="color: #a9a9a9">.</span><span style="color: #000000">OldFilePath</span><span style="color: #000000">}</span>
                <span style="color: #000000">}</span>
            <span style="color: #000000">}</span>            

            <span style="color: #ff4500">$SkippedChangeAction</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">{</span>
                <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Skipped</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">CurrentFilePath</span>            

                <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Exception</span> <span style="color: #a9a9a9">-ne</span> <span style="color: #ff4500">$null</span><span style="color: #000000">)</span>
                <span style="color: #000000">{</span>
                    <span style="color: #0000ff">Write-Error</span> <span style="color: #8b0000">'['</span> <span style="color: #8a2be2">+</span> <span style="color: #8b0000">"$($event.SourceEventArgs.Exception.Message)"</span> <span style="color: #8a2be2">+']'</span>
                <span style="color: #000000">}</span>
            <span style="color: #000000">}</span>            

            <span style="color: #006400"># Create source provider and register change events for it</span>            

            <span style="color: #ff4500">$sourceProvider</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span> <span style="color: #8a2be2">Microsoft.Synchronization.Files.FileSyncProvider</span> <span style="color: #000080">-ArgumentList</span> <span style="color: #ff4500">$SourcePath</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$options</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$SourceProvider</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">AppliedChange</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$AppliedChangeAction</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$SourceProvider</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">SkippedChange</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$SkippedChangeAction</span>             

            <span style="color: #ff4500">$ApplyChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$SourceApplyChangeJob</span>            

            <span style="color: #006400"># Create destination provider and register change events for it</span>
            <span style="color: #ff4500">$destinationProvider</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span> <span style="color: #8a2be2">Microsoft.Synchronization.Files.FileSyncProvider</span> <span style="color: #000080">-ArgumentList</span> <span style="color: #ff4500">$DestinationPath</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">,</span> <span style="color: #ff4500">$options</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$destinationProvider</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">AppliedChange</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$AppliedChangeAction</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$destinationProvider</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">SkippedChange</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$SkippedChangeAction</span>            

            <span style="color: #ff4500">$ApplyChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$DestApplyChangeJob</span>            

            <span style="color: #006400"># Use scriptblocks for the SyncCallbacks for conflicting items.</span>
            <span style="color: #ff4500">$ItemConflictAction</span> <span style="color: #a9a9a9">=</span>   <span style="color: #000000">{</span>
                <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SetResolutionAction</span><span style="color: #000000">(</span><span style="color: #008080">[Microsoft.Synchronization.ConflictResolutionAction]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">SourceWins</span><span style="color: #000000">)</span>
                <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Conflicted</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationChange</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ItemId</span>
            <span style="color: #000000">}</span>
            <span style="color: #ff4500">$ItemConstraintAction</span> <span style="color: #a9a9a9">=</span> <span style="color: #000000">{</span>
                <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SetResolutionAction</span><span style="color: #000000">(</span><span style="color: #008080">[Microsoft.Synchronization.ConstraintConflictResolutionAction]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">SourceWins</span><span style="color: #000000">)</span>
                <span style="color: #008080">[string[]]</span><span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Constrained</span> <span style="color: #a9a9a9">+=</span> <span style="color: #ff4500">$event</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceEventArgs</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationChange</span><span style="color: #a9a9a9">.</span><span style="color: #000000">ItemId</span>
            <span style="color: #000000">}</span>            

            <span style="color: #006400">#Configure the events for conflicts or constraints for the source and destination providers</span>
            <span style="color: #ff4500">$destinationCallbacks</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$destinationProvider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationCallbacks</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$destinationCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConflicting</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConflictAction</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$destinationCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConstraint</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConstraintAction</span>             

            <span style="color: #ff4500">$sourceCallbacks</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$SourceProvider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationCallbacks</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$sourceCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConflicting</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConflictAction</span>
            <span style="color: #ff4500">$AppliedChangeJobs</span> <span style="color: #a9a9a9">+=</span> <span style="color: #0000ff">Register-ObjectEvent</span> <span style="color: #000080">-InputObject</span> <span style="color: #ff4500">$sourceCallbacks</span> <span style="color: #000080">-EventName</span> <span style="color: #8a2be2">ItemConstraint</span> <span style="color: #000080">-Action</span> <span style="color: #ff4500">$ItemConstraintAction</span>             

            <span style="color: #006400"># Create the agent that will perform the file sync</span>
            <span style="color: #ff4500">$agent</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span>  <span style="color: #8a2be2">Microsoft.Synchronization.SyncOrchestrator</span>
            <span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">LocalProvider</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$sourceProvider</span>
            <span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">RemoteProvider</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$destinationProvider</span>            

            <span style="color: #006400"># Upload changes from the source to the destination.</span>
            <span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Direction</span> <span style="color: #a9a9a9">=</span> <span style="color: #008080">[Microsoft.Synchronization.SyncDirectionOrder]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">Upload</span>            

            <span style="color: #0000ff">Write-Host</span> <span style="color: #8b0000">"Synchronizing changes from $($sourceProvider.RootDirectoryPath) to replica: $($destinationProvider.RootDirectoryPath)"</span>
            <span style="color: #ff4500">$agent</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Synchronize</span><span style="color: #000000">(</span><span style="color: #000000">)</span><span style="color: #000000">;</span>
        <span style="color: #000000">}</span>
        <span style="color: #00008b">finally</span>
        <span style="color: #000000">{</span>
            <span style="color: #006400"># Release resources.</span>
            <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$sourceProvider</span> <span style="color: #a9a9a9">-ne</span> <span style="color: #ff4500">$null</span><span style="color: #000000">)</span> <span style="color: #000000">{</span><span style="color: #ff4500">$sourceProvider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Dispose</span><span style="color: #000000">(</span><span style="color: #000000">)</span><span style="color: #000000">}</span>
            <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$destinationProvider</span> <span style="color: #a9a9a9">-ne</span> <span style="color: #ff4500">$null</span><span style="color: #000000">)</span> <span style="color: #000000">{</span><span style="color: #ff4500">$destinationProvider</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Dispose</span><span style="color: #000000">(</span><span style="color: #000000">)</span><span style="color: #000000">}</span>
        <span style="color: #000000">}</span>
    <span style="color: #000000">}</span>            

    <span style="color: #006400"># Set options for the synchronization session. In this case, options specify</span>
    <span style="color: #006400"># that the application will explicitly call FileSyncProvider.DetectChanges, and</span>
    <span style="color: #006400"># that items should be moved to the Recycle Bin instead of being permanently deleted.</span>            

    <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">ExplicitDetectChanges</span>
    <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">-bor</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">RecycleDeletedFiles</span>
    <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">-bor</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">RecyclePreviousFileOnUpdates</span>
    <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">=</span> <span style="color: #ff4500">$options</span> <span style="color: #a9a9a9">-bor</span> <span style="color: #008080">[Microsoft.Synchronization.Files.FileSyncOptions]</span><span style="color: #a9a9a9">::</span><span style="color: #000000">RecycleConflictLoserFiles</span>
<span style="color: #000000">}</span>
<span style="color: #00008b">process</span>
<span style="color: #000000">{</span>
    <span style="color: #ff4500">$filter</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span> <span style="color: #8a2be2">Microsoft.Synchronization.Files.FileSyncScopeFilter</span>
    <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$FileNameFilter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">count</span> <span style="color: #a9a9a9">-gt</span> <span style="color: #800080">0</span><span style="color: #000000">)</span>
    <span style="color: #000000">{</span>
       <span style="color: #ff4500">$FileNameFilter</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">ForEach-Object</span> <span style="color: #000000">{</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">FileNameExcludes</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Add</span><span style="color: #000000">(</span><span style="color: #ff4500">$_</span><span style="color: #000000">)</span> <span style="color: #000000">}</span>
    <span style="color: #000000">}</span>
    <span style="color: #00008b">if</span> <span style="color: #000000">(</span><span style="color: #ff4500">$SubdirectoryNameFilter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">count</span> <span style="color: #a9a9a9">-gt</span> <span style="color: #800080">0</span><span style="color: #000000">)</span>
    <span style="color: #000000">{</span>
       <span style="color: #ff4500">$SubdirectoryNameFilter</span> <span style="color: #a9a9a9">|</span> <span style="color: #0000ff">ForEach-Object</span> <span style="color: #000000">{</span> <span style="color: #ff4500">$filter</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SubdirectoryExcludes</span><span style="color: #a9a9a9">.</span><span style="color: #000000">Add</span><span style="color: #000000">(</span><span style="color: #ff4500">$_</span><span style="color: #000000">)</span> <span style="color: #000000">}</span>
    <span style="color: #000000">}</span>            

    <span style="color: #006400"># Perform the detect changes operation on the two file locations</span>
    <span style="color: #0000ff">Get-FileSystemChange</span> <span style="color: #ff4500">$SourcePath</span> <span style="color: #ff4500">$filter</span> <span style="color: #ff4500">$options</span>
    <span style="color: #0000ff">Get-FileSystemChange</span> <span style="color: #ff4500">$DestinationPath</span> <span style="color: #ff4500">$filter</span> <span style="color: #ff4500">$options</span>            

    <span style="color: #006400"># Reporting Object - using the global scope so that it can be updated by the event scriptblocks.</span>
    <span style="color: #ff4500">$global:FileSyncReport</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">New-Object</span> <span style="color: #8a2be2">PSObject</span> <span style="color: #a9a9a9">|</span>
        <span style="color: #0000ff">Select-Object</span> <span style="color: #8a2be2">SourceStats</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">DestinationStats</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Created</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Deleted</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Overwritten</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Renamed</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Skipped</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Conflicted</span><span style="color: #a9a9a9">,</span> <span style="color: #8a2be2">Constrained</span>            

    <span style="color: #006400"># We don't need to pass any filters here, since we are using the file detection that was previously completed.</span>
    <span style="color: #006400"># this will only </span>
    <span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">SourceStats</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">Invoke-OneWayFileSync</span> <span style="color: #000080">-SourcePath</span> <span style="color: #ff4500">$SourcePath</span> <span style="color: #000080">-DestinationPath</span> <span style="color: #ff4500">$DestinationPath</span> <span style="color: #000080">-Filter</span> <span style="color: #ff4500">$null</span> <span style="color: #000080">-Options</span> <span style="color: #ff4500">$options</span>
    <span style="color: #ff4500">$global:FileSyncReport</span><span style="color: #a9a9a9">.</span><span style="color: #000000">DestinationStats</span> <span style="color: #a9a9a9">=</span> <span style="color: #0000ff">Invoke-OneWayFileSync</span> <span style="color: #000080">-SourcePath</span> <span style="color: #ff4500">$DestinationPath</span> <span style="color: #000080">-DestinationPath</span> <span style="color: #ff4500">$SourcePath</span> <span style="color: #000080">-Filter</span> <span style="color: #ff4500">$null</span> <span style="color: #000080">-Options</span> <span style="color: #ff4500">$options</span>            

    <span style="color: #006400"># Write result to pipeline</span>
    <span style="color: #0000ff">Write-Output</span> <span style="color: #ff4500">$global:FileSyncReport</span>
<span style="color: #000000">}</span>

<h3 class="PowerShellColorizedScript"><span style="color: #000000"><a href="http://download.usepowershell.com/Invoke-SyncFrameworkSample.zip" target="_blank">Download Invoke-SyncFrameworkSample.zip</a></span>


