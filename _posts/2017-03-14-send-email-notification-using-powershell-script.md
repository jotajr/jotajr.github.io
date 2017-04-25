---
layout: post
title: Send email notification using PowerShell script
description: Simple way to automate or send email notification using PowerShell script with SMTP client.
keywords: send email notification, getting notified, email sender, sending email with powershell, email notify with smtp
tags: [PowerShell]
comments: true
---

At work, I have been creating many internal tools or writing various type of scripts in order to automate some tasks or to get the tasks easily be done by automation processes. One of the common tasks in automation is getting notified when a certain task is complete or failed. This can be implemented on any platform or medium for particular notification message to be delivered, but the most common one is via email. On Windows, batch and PowerShell scripts are the best friends to any automation process.

Assuming you are executing certain task that requires you to get notified via email, the simplest way you do is by executing a PowerShell script to send you an email notification. This is what usually I do when I just want to get a simple email notification for my tasks. But for reporting-like email where it requires data interpreting or processing that can generate outputs like table or chart, I usually will use my own internal developed tool or program that is created for that purpose.

### Script

This script is very straightforward and simply send email notification using SMTP client. Not really a total complete solution though, but you will get the idea how it works to get started with it.

**EmailNotification.ps1**

```powershell
# SETTING UP NECESSARY PARAMETERS
$emailTo = "user1@email.com,user2@example.com" # Separate by comma for multiple email addresses
$emailFrom = "sender@email.com"
$emailFromDisplayName = "Auto Email Notification"
$smtpServer = "smtp.yourserver.com"
$smtpPort = 0 # 0 = not in use
$enableSSL = $false
$useDefaultCredentials = $true
$username = "" # Provide this when $useDefaultCredentials = $false
$password = "" # Provide this when $useDefaultCredentials = $false
$useHTML = $true
$priority = [System.Net.Mail.MailPriority]::High

# ACQUIRING DATA OR INFO, INTERPRET IT AND PROCESS THE RESULT
# Many things you can do here...

# CREATING MESSAGE BODY
# Generally I use HTML for email message body.
$html_var1 = "..."
$html_var2 = "..."
$html_var3 = "..."
$emailBody = $html_var1 + $html_var2 + $html_var3

# CREATING EMAIL SUBJECT
# Sometimes I decide the email subject at last, or auto-generate it.
$emailSubject = "..."

# SENDING EMAIL...
# You might want to do checking/validation beforehand on necessary variables
$mailMessage = New-Object System.Net.Mail.MailMessage
if ($emailFromDisplayName -eq "" -or $emailFromDisplayName -eq $null) {
    $mailMessage.From = New-Object System.Net.Mail.MailAddress($emailFrom)
} else {
    $mailMessage.From = New-Object System.Net.Mail.MailAddress($emailFrom, $emailFromDisplayName)
}
$mailMessage.To.Add($emailTo)
$mailMessage.Subject = $emailSubject
$mailMessage.Body = $emailBody
$mailMessage.IsBodyHtml = $useHTML
$mailMessage.Priority = $priority

$smtp = $null
if ($smtpPort -ne 0) {
    $smtp = New-Object System.Net.Mail.SmtpClient($smtpServer, $smtpPort)
} else {
    $smtp = New-Object System.Net.Mail.SmtpClient($smtpServer)
}
$smtp.EnableSsl = $enableSSL
$smtp.DeliveryMethod = [System.Net.Mail.SmtpDeliveryMethod]::Network

if ($useDefaultCredentials -eq $true) {
    $smtp.UseDefaultCredentials = $true
} else {
    $smtp.UseDefaultCredentials = $false
    $smtp.Credentials = New-Object System.Net.NetworkCredential($username, $password)
}

try {
    Write-Output ("Sending email notification...")
    $smtp.Send($mailMessage)
    Write-Output ("Sending email notification --> Complete")
} catch {
    Write-Output ("Sending email notification --> Error")
    Write-Warning ('Failed to send email: "{0}"', $_.Exception.Message)
}
```

### Make as a function

Making the script as a function gives you a better flexibility where you can execute the script with a set of assignable variables/parameters as the arguments. You may do something like this:

```powershell
[CmdletBinding(SupportsShouldProcess=$true, ConfirmImpact='Medium')]
[Alias()]
[OutputType([int])]
Param
(
    # Separate by comma for multiple email addresses
    [Parameter(Mandatory=$true, Position=0)]
    [ValidateNotNullOrEmpty()]
    [string]$EmailTo = "",

    [Parameter(Position=1)]
    [ValidateNotNullOrEmpty()]
    [string]$EmailFrom = "sender@example.com"

    # ...more parameters that suit your needs
    # continue here...
)


function EmailNotification
{
    [CmdletBinding(SupportsShouldProcess=$true, ConfirmImpact='Medium')]
    [Alias()]
    [OutputType([int])]
    Param
    (
        [Parameter(Mandatory=$true, Position=0)]
        [ValidateNotNullOrEmpty()]
        [string]$EmailTo = "",

        [Parameter(Position=1)]
        [ValidateNotNullOrEmpty()]
        [string]$EmailFrom = "sender@example.com",

        [Parameter(Position=2)]
        [AllowEmptyString()]
        [string]$EmailFromDisplayName = "Auto-build Notification System",

        [Parameter(Position=3)]
        [ValidateNotNullOrEmpty()]
        [string]$SmtpServer = "smtp.example.com"

        # ...more parameters that suit your needs
        # continue here...
    )

    # YOUR MAIN SCRIPT GOES HERE...
    # Process the paramaters, creating message body and sending the email...

}

if (-not ($myinvocation.line.Contains("`$here\`$sut"))) {
    EmailNotification -EmailTo $EmailTo -EmailFrom $EmailFrom # ...and more parameters here...
}
```
