---
layout: post
title: Threat Intelligence - Friday Overtime
date: 2024-09-20 12:52
author: yurenjoeysie
comments: true
categories: [Cyber Threat Intelligence, CyberChef, Incident Response, Indicators of Compromise, Threat Intelligence, TryHackMe Challenge Rooms, VirusTotal]
---
<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">On a Friday evening at PandaProbe Intelligence, a notification on the CTI platform indicates a new ticket from SwiftSpend Finance, raising concerns about potential malware threats. Despite it being the weekend, immediate attention is required due to the seriousness of the situation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Click <a href="https://tryhackme.com/r/room/fridayovertime">Friday Overtime</a> to begin this challenge.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">As the only CTI Analyst on duty, we took several crucial actions to address the malware threat. We began by securely downloading the malware samples from the ticket and conducted a preliminary analysis using automated tools for an initial assessment. Following this, we performed a detailed manual review to understand the malware's behavior and communication patterns. We then correlated our findings with global threat intelligence databases to identify any known signatures. Finally, we compiled a comprehensive report with mitigation and recovery recommendations for SwiftSpend Finance.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once the virtual machine has finished booting up, the web page will automatically open. If you see a "Connection Refused" message, please wait a few more minutes for the backend processes to complete.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1088,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-353.png?w=827" alt="" class="wp-image-1088" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Who shared the malware samples?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After logging in with the provided credentials, we will see an email titled "Urgent: Malicious Malware Artifacts Detected." Once we read through the email, we can find the sender at the bottom.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1090,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-354.png?w=827" alt="" class="wp-image-1090" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the SHA1 hash of the file "pRsm.dll" inside samples.zip?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In the same email, there is a malware sample file named samples.zip. To access the file pRsm[.]dll inside samples.zip, use the unzip command to extract its contents.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">Note: This file is password-protected with the password (Panda321!), which is provided in the email.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1091,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-355.png?w=1024" alt="" class="wp-image-1091" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Next, run the sha1sum command on the pRsm[.]dll file to obtain its hash value.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1093,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-356.png?w=1024" alt="" class="wp-image-1093" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Which malware framework utilizes these DLLs as add-on modules?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To gather more information about the malware associated with these DLLs, we can paste the hash value of pRsm[.]dll into <a href="https://www.virustotal.com/gui/file/2c0cfe2f4f1e7539b4700e1205411ec084cbc574f9e4710ecd4733fbf0f8a7dc/community">VirusTotal</a>. In the community tab, we will find a report mentioning this DLL.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1094,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-357.png?w=1024" alt="" class="wp-image-1094" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">According to the article <a href="https://symantec-enterprise-blogs.security.com/threat-intelligence/apt-attacks-telecoms-africa-mgbot">APT Actor Targets Telecoms Company in Africa</a>, DLLs are used by the malware framework for various purposes.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">Cbmrpa[.]dll: Screen and clipboard grabber<br>pRsm[.]dll: Audio capture<br>mailfpassword[.]dll: Outlook and Foxmail credentials stealer<br>qmsdp[.]dll: QQ messages infostealer.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1096,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-358.png?w=1024" alt="" class="wp-image-1096" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Which MITRE ATT&amp;CK Technique is linked to using pRsm.dll in this malware framework?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">As mentioned earlier, pRsm[.]dll is used for audio capture. To explore its association with MITRE ATT&amp;CK, we can search for either "pRsm[.]dll" or "audio capture" within the <a href="https://attack.mitre.org/techniques/T1123/">MITRE ATT&amp;CK</a> framework. We found a technique specifically named "Audio Capture" in the framework.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1097,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-359.png?w=1024" alt="" class="wp-image-1097" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the defanged URL of the malicious download location first seen on 2020-11-02?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I found another article titled <a href="https://www.welivesecurity.com/2023/04/26/evasive-panda-apt-group-malware-updates-popular-chinese-software/">Evasive Panda APT Group Delivers Malware via Updates for Popular Chinese Software</a> under the Community tab on <a href="https://www.virustotal.com/gui/file/2c0cfe2f4f1e7539b4700e1205411ec084cbc574f9e4710ecd4733fbf0f8a7dc/community">VirusTotal</a>, which discusses this malware. In the Technical Analysis section, we will find details about the URL from which the download originated.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1098,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-360.png?w=1024" alt="" class="wp-image-1098" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once we have the hosting address, simply input it into <a href="https://cyberchef.org/#recipe=Defang_URL(true,true,true,'Valid%20domains%20and%20full%20URLs')&amp;input=aHR0cDovL3VwZGF0ZS5icm93c2VyLnFxWy5dY29tL3FtYnMvUVEvUVFVcmxNZ3JfUVE4OF80Mjk2LmV4ZQ">CyberChef</a> to generate the defanged URL.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1099,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-361.png?w=1024" alt="" class="wp-image-1099" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the CyberChef defanged IP address of the C&amp;C server first detected on 2020-09-14 using these modules?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Within the same article &nbsp;In the Network section, the article provides the IP address of the C2 server that the malware contacts from the affected machine.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1101,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-362.png?w=1024" alt="" class="wp-image-1101" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the SHA1 hash of the spyagent family spyware hosted on the same IP targeting Android devices on November 16, 2022?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After searching for the C2 server’s IP address on <a href="https://www.virustotal.com/gui/ip-address/122.10.90.12/relations">VirusTotal</a>, navigate to the Relations tab. In the Communicating Files section, we will find a malicious file categorized as an Android file type.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1102,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-363.png?w=1024" alt="" class="wp-image-1102" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After clicking on the <a href="https://www.virustotal.com/gui/file/bbef5975a0483220cfec379c44a487ed4146e0af9205f00dbc0eb53de8a63533/details">filename</a>, we will be directed to another page containing detailed information about this malicious file. The SHA-1 hash value can be found under the Details tab.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1104,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-364.png?w=1024" alt="" class="wp-image-1104" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
