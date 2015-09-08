---
layout: post
title: "How To Send E-Mail From PowerShell"
date: 2009-03-02 09:30
author: steven.murawski@gmail.com
comments: true
categories: [-NET Framework, Base Class Libraries, PowerShell, PowerShell Version 1]
tags: []
---


In my [Exploring the .NET Framework series](http://static.squarespace.com/static/50a13c5be4b039333cb95a3b/50acf4c0e4b0c945709cfb5c/50acf517e4b0c945709cff39/1353512215537/?format=original), I introduced the <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx#" target="_blank">System.Net.Mail.MailMessage</a> class.  Being able to create a MailMessage object is all well and good, but if you can’t send it, it’s really not helpful.


### Updated  - This mainly applies to V1 of PowerShell. V2 has a built in cmdlet for this purpose.




To send an email from <a href="http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx" target="_blank">PowerShell</a> using the .NET Framework, you can use the <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.smtpclient.aspx#" target="_blank">System.Net.Mail.SMTPClient</a> class.



First, you need to create your MailMessage



PS C:\scripts\PowerShell&gt; $message = New-Object System.Net.Mail.MailMessage –ArgumentList steve@usepowershell.com, nobody@adomain.com, 'This is a test', "I’m going to send an email via PowerShell"</pre>

    
    If you need to add an attachment, you can add one with the <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.attachment.aspx" target="_blank">System.Net.Mail.Attachment</a> class.
    

    
    To create the attachment, we’ll create an use an overload of the Attachment class that allows us to specify a filename and a MIME type (the MIME types are available in several enumerations:  <a href="http://msdn.microsoft.com/en-us/library/system.net.mime.mediatypenames.application.aspx" target="_blank">System.Net.Mime.MediaTypeNames.Application</a>, <a href="http://msdn.microsoft.com/en-us/library/system.net.mime.mediatypenames.image.aspx" target="_blank">System.Net.Mime.MediaTypeNames.Image</a>, and <a href="http://msdn.microsoft.com/en-us/library/system.net.mime.mediatypenames.text.aspx" target="_blank">System.Net.Mime.MediaTypeNames.Text</a> – I’ve had a bit of trouble accessing those enumerations via PowerShell, but you can specify the name of the enumeration value in a string).
    
<pre>PS C:\scripts\PowerShell&gt; $attachment = New-Object System.Net.Mail.Attachment –ArgumentList 'c:\docs\test.xls’, ‘Application/Octet’

PS C:\scripts\PowerShell&gt; $message.Attachments.Add($attachment)</pre>

    
    After we have our email and optional attachment, we’ll create the SMTP client and have it configured to use the SMTP server of our choice.
    

    
    The constructor of the SMTPClient class that we will use is
    

    
    SmtpClient(
String host,
)
    
<pre>PS C:\scripts\PowerShell&gt; $smtp = New-Object System.Net.Mail.SMTPClient –ArgumentList 10.10.10.15</pre>

    
    All that is left is sending our email..
    
<pre>PS C:\scripts\PowerShell&gt; $smtp.Send($message)</pre>

    
    Have fun!
    

    
    NOTE:
    

    
    There is a shortcut to creating the MailMessage object like we did.. there is an overload of Send where you can pass in four strings (from, to, subject, and message).  The SMTPClient handles the creation of the MailMessage in the background.
    

    
    A faster method would be
    
<pre>PS C:\scripts\PowerShell&gt; $smtp = New-Object System.Net.Mail.SMTPClient –ArgumentList 10.10.10.15

PS C:\scripts\PowerShell&gt; $smtp.Send('steve@usepowershell.com', 'nobody@adomain.com', 'This is a test', "I'm going to send an email via PowerShell")

