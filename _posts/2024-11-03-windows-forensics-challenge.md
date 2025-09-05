---
layout: post
title: Windows Forensics Challenge
date: 2024-11-03 18:37
author: yurenjoeysie
comments: true
categories: [Command and Control, Endpoint Monitoring, Endpoint Security, Event ID, Event Viewer, File System, Hash, Incident Response, Indicators of Compromise, Linux Forensics, Malware Analysis, Network Security, PowerShell, Process Explorer, Registry Key, TryHackMe Challenge Rooms, Windows Event Logs, Windows Forensic, Windows Process, Windows System]
---
<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size">Click <a href="https://tryhackme.com/r/room/investigatingwindows">here</a> to enter the challenge room on Try Hack Me</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">This challenge involves investigating a previously compromised Windows machine, presenting several key obstacles.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Tasks</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#1 What’s the version and year of the windows machine?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By going to "About" in Windows settings, we can find the machine's version and year.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3623,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image.png?w=920" alt="" class="wp-image-3623" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#2 Which user logged in last?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To identify successful logon processes in the Windows Event Log, we will filter for Event ID 4624. Then, we'll check the Security event date before our logon session (2024/11/2) to find the last logged-in user.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: Event ID 4625 indicates a failed logon attempt.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3625,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-1.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-1.png?w=941" alt="" class="wp-image-3625" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#3 When did John log onto the system last?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To determine when the user "John" last logged onto the system, use utility net.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>C:\&gt; net user &lt;username&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3627,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-2.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-2.png?w=1024" alt="" class="wp-image-3627" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: "Net" is a CLI tool in Windows used for network-related tasks. It can manage network resources, user accounts, and share files or printers.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#4 What IP does the system connect to when it first starts?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I first searched for the DHCP server IP address, which was incorrect. Then I navigated to the SOFTWARE Registry Key (Microsoft\Windows\CurrentVersion\Run), which contains entries for executables that run automatically at startup, and found the correct IP address.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3630,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-3.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-3.png?w=938" alt="" class="wp-image-3630" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: This command executes p.exe with the -s flag, which typically indicates a silent or specific mode of operation. It runs the net user command on a remote machine at IP address 10.34.2.3 and outputs the result to a text file named o2.txt in the same C:\TMP directory.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#5 What two accounts had administrative privileges (other than the Administrator user)?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We can list all accounts in the Administrators group using the PowerShell:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>PS C:\&gt; Get-LocalGroupMember -Group "Administrators"</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3633,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-5.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-5.png?w=1024" alt="" class="wp-image-3633" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#6 What's the name of the scheduled task that is malicious?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Upon reviewing the scheduled tasks in Task Scheduler, I identified three that appear suspicious.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3634,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-1.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-1.png?w=1024" alt="" class="wp-image-3634" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#7 What file was the task trying to run daily?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The previous screenshot shows a task scheduled to run daily. Clicking on the task reveals more details.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3635,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-2.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-2.png?w=639" alt="" class="wp-image-3635" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#8 What port did this file listen locally for?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: -l indicates listen.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#9 When did Jenny last logon?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Regarding the third task, we used it to identify John's last logon time.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>C:\&gt; net user &lt;username&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3637,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-3.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-3.png?w=1024" alt="" class="wp-image-3637" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#10 At what date did the compromise take place?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Reviewing the creation dates of the scheduled tasks suggests that this is likely the initial compromise date, as attackers typically implement persistence mechanisms after breaching a system.,</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3638,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-4.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-4.png?w=863" alt="" class="wp-image-3638" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#11 During the compromise, at what time did Windows first assign special privileges to a new logon?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">While investigating the security logs from the compromise period (3/2/2019, 16:00 – 17:00) and Event ID 4672, which records users logging on with special privileges, we tracked down the event related to the special logon.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3639,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-5.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-5.png?w=1024" alt="" class="wp-image-3639" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The Event ID 4672 signifies that a logon session was created with elevated privileges, and in this case, it was initiated by the SYSTEM account. The PrivilegeList shows the specific privileges assigned, which include important rights like SeDebugPrivilege and SeImpersonatePrivilege, indicating that the logon was granted administrator control over the system.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#12 What tool was used to get Windows passwords?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">While investigating scheduled tasks in Task Scheduler, I noticed a task named "GameOver" that executes mim.exe every five minutes. Analyzing the arguments reveals it is associated with a known password extraction tool.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3641,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-6.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-6.png?w=1024" alt="" class="wp-image-3641" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The command extract and display stored logon credentials from the Windows memory, specifically focusing on the password information of logged-on users. Then it stores the extracted credentials to o.txt</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#13 What was the attacker’s external control and command servers IP?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">While investigating task #4, I noticed an unusual IP address linked to google.com in the DNS host file (C:\Windows\System32\drivers\etc), likely indicating an external C2 server.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3642,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-7.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-7.png?w=1024" alt="" class="wp-image-3642" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#14 What was the extension name of the shell uploaded via the server’s website?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After investigating another suspicious directory (C:\inetpub\wwwroot), I found several .jsp files likely associated with file extraction. Which the attacker leveraged the JSP file on the victim machine to facilitate data exfiltration to the external server.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3643,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-8.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-8.png?w=810" alt="" class="wp-image-3643" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: According to <a href="https://www.geeksforgeeks.org/introduction-to-jsp/">Introduction to JSP</a> on Geeksforgeeks, JSP is used in Java for building dynamic web applications. It combines HTML and Java code, allowing developers to create interactive web pages. When a JSP page is requested, it is first converted into a Java program (a servlet) by the server before being sent to the user.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#15 What was the last port the attacker opened?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Following the hint to investigate the firewall, we found several rules allowing unusual local ports to remain open. And one of them is the answer.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3645,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-9.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-4-9.png?w=1024" alt="" class="wp-image-3645" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#16 Check for DNS poisoning, what site was targeted?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Refer to task #13 for the domain affected by DNS poisoning.</p>
<!-- /wp:paragraph -->
