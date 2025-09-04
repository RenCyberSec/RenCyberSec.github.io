---
layout: post
title: Registry Explorer - Secret Recipe
subtitle: Registry artifcats exmination for evidence
date: 2024-09-28 22:12
author: Ren Sie
comments: true
category: digital-forensics
---

{: .box-success}
 Refer to [Registry4n6](https://tryhackme.com/r/room/registry4n6) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Jasmine owns a renowned coffee shop, Coffely, celebrated for its unique recipe, which she keeps exclusively on her work laptop. Recently, she sought help from James in the IT department to fix her laptop. However, there are suspicions that he may have copied the secret recipe. James's machine has been confiscated and examined, but no traces of the recipe were found. The security department has retrieved important registry artifacts from his device and has tasked you with examining these artifacts to determine whether any secret files exist on his machine.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">The Computer Name of the Machine found in the registry?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To find the name of the machine, we need to check the System Registry using Registry Explorer. We can either use the search function for "ComputerName" or navigate through the following path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2246,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-815.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-815.png?w=1024" alt="" class="wp-image-2246" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">When was the Admin account created on this machine?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The SAM Registry holds information about the users on the machine, include the account creating date. We can use the same method as before on Registry Explorer: either use the search function for "User" or navigate through the following path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">SAM\Domains\Account\Users</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2247,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-816.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-816.png?w=1024" alt="" class="wp-image-2247" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the RID associated with the Admin account?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Referring to the previous screenshot, we can identify the RID associated with the administrator account.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** A User ID is a general term for any identifier related to a user account, while a RID is a specific numeric part of a Security Identifier (SID) that uniquely identifies a user or group within a domain. E.g., in the SID S-1-5-21-1004336348-1177238915-682003330-500, the RID is 500. Additionally, RID 500 is always linked to the built-in Administrator account for either a domain or a local system.

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">How many User accounts were observed on this machine?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Referring to the previous screenshot, we can identify the available user accounts on the machine by examining the information shown in the total rows.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the Account Name associated with RID 1013. ?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Referring to the previous screenshot, we can identify the user account name associated with RID 1013.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the VPN connection this host connected to?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To identify the network connection history, we will examine the Software Registry in Registry Explorer. We can do this either by using the search function for "NetworkList" or by following the path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2248,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-817.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-817.png?w=1024" alt="" class="wp-image-2248" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">When was the first VPN connection observed?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Referring to the previous screenshot, we can identify the first VPN connection that was made.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the path of the third shared folders on the machine?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To identify the shared directory, we will examine the LanmanServer/shares in Registry Explorer. We can do this either by using the search function for "Shares" or by following this path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">SYSTEM\CurrentControlSet\Services\LanmanServer\Shares</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2250,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-818.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-818.png?w=1024" alt="" class="wp-image-2250" /></a></figure>
<!-- /wp:image -->

{: .box-note}
**Note:** LanmanServer is a Windows service that enables the sharing of files and printers over a network using the Server Message Block (SMB) protocol.

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the Last DHCP IP assigned to this host?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To learn the IP address, we will investigate the System Registry on Registry Explorer. We can do this either by using the search function for "interfaces" or by following this path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">However, the results didn’t include a timeline, so I had to infer that either the top or bottom entry is the last DHCP IP address assigned to the host.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2251,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-819.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-819.png?w=1024" alt="" class="wp-image-2251" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the filename of secret coffee recipe that's been accessed?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Since the question states that the suspect accessed the secret coffee recipe, we will investigate the RecentDocs in Registry Explorer. We can do this either by using the search function for "RecentDocs" or by following this path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2253,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-820.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-820.png?w=1024" alt="" class="wp-image-2253" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What command was run to enumerate the network interfaces?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">RunMRU (Most Recently Used) keeps a list of commands that users have entered in the Run dialog. You can access it by searching for "RunMRU" or by following this path:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT/Software/Microsoft/Windows/CurrentVersion/Explorer/RunMRU</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** The command pnputil /enum-interfaces lists all network interfaces and their properties on the machine, providing details such as interface names, types, and statuses.

<!-- wp:image {"id":2254,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-821.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-821.png?w=1024" alt="" class="wp-image-2254" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the name of the network utility the user searched for to transfer files in the file explorer?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To find the searched keywords in File Explorer, we need to investigate the WordWheelQuery under the Software Registry on the Registry Explorer. Based on the results, netcat is the only keyword related to the network utility.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2256,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-822.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-822.png?w=1024" alt="" class="wp-image-2256" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the recent text file opened by the suspect?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">We will apply the same method as we did for this question (<strong>What is the filename of secret coffee recipe that's been accessed?</strong>) to locate the text file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2257,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-823.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-823.png?w=886" alt="" class="wp-image-2257" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">How many times was PowerShell executed on this host?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To learn the executed times of an program, we can check the UserAssist, which store details like which programs were launched, when they were launched, and how often. Unfortunately, we can only locate it through the path, not the search function. I used the search in the screenshot for better clarity.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2260,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-824.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-824.png?w=1024" alt="" class="wp-image-2260" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">The suspect also executed a network monitoring tool. What is the name of the tool?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will apply the same method as we did for this question (<strong>How many times was Powershell executed on this host?</strong>) to discover the network monitoring tool.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2261,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-825.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-825.png?w=1024" alt="" class="wp-image-2261" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">How many seconds was ProtonVPN executed, based on the Registry Hives?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will apply the same method as we did for this question (<strong>How many times was Powershell executed on this host?</strong>) to determine how long ProtonVPN has been running.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2263,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-826.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-826.png?w=1024" alt="" class="wp-image-2263" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the full path from which Everything.exe was executed?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To find the complete path of an application that has been run, we can check the Background Activity Monitor (BAM), which tracks the activity of background applications. Unfortunately, we can only locate it through the path, not the search function. I used the search in the screenshot for better clarity.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2264,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-827.png?w=1024" alt="" class="wp-image-2264" /></figure>
<!-- /wp:image -->

