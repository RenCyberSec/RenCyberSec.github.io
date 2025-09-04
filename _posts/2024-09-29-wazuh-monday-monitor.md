---
layout: post
title: Wazuh - Monday Monitor
Subtitle: Discovered persistent threat on compromised endpoint with EDR (Wazuh)
date: 2024-09-29 17:41
author: Ren Sie
comments: true
category: threat-hunting
---

{: .box-success}
 Refer to [Mondaymonitor](https://tryhackme.com/r/room/mondaymonitor) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Swiftspend Finance, the coolest fintech company around, is enhancing its cybersecurity to protect against digital threats and keep customers safe. Led by the tech-savvy Senior Security Engineer John Sterling, Swiftspend is focusing on improving endpoint monitoring with <strong>Wazuh</strong> and <strong>Sysmon</strong>. They’ve conducted tests to evaluate their security measures, and now they need your expertise.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The tests took place on April 29, 2024, from 12:00 PM to 8:00 PM. Your task is to analyze the logs for any suspicious processes or unusual network connections. Your mission is to uncover insights and help refine Swiftspend’s defenses.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once logged in, navigate to the Security Events module and use the saved query <strong>Monday_Monitor</strong> to access the logs. As well as selecting the period according to the description.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2273,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-828.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-828.png?w=709" alt="" class="wp-image-2273" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2274,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-829.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-829.png?w=709" alt="" class="wp-image-2274" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Initial access was established using a downloaded file. What is the file name saved on the host?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Given that the question referenced 'download file,' I used the term '<strong>http</strong>' in the search head.<br>I identified a PowerShell script that downloads a file (SwiftSpend_Financial_Expenses.xlsm) from localhost and saves it as PhishingAttachment.xlsm in the TEMP\ directory.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2276,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-830.png?w=945" alt="" class="wp-image-2276" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the full command run to create a scheduled task?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To monitor the scheduled task, I utilized <strong>schtasks.exe</strong> on the search head.<br>I discovered that the script configures both a Windows registry entry and a scheduled. It is set to execute a PowerShell command daily at 12:34 PM, which will decode and execute the registry entry.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** schtasks.exe is a Windows utility used for creating, deleting, configuring, and displaying scheduled tasks.

<!-- wp:image {"id":2278,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-831.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-831.png?w=945" alt="" class="wp-image-2278" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What time is the scheduled task meant to run?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">According to the previous screenshot, the scheduled task is set to activate daily at 12:34 PM.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What was encoded?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The previous screenshot indicates that the string is encoded in Base64 (cGluZyB3d3cueW91YXJldnVsbmVyYWJsZS50aG0=). By using CyberChef to decode this string, I obtain the original content.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2280,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-832.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-832.png?w=945" alt="" class="wp-image-2280" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What password was set for the new user account?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I utilized the search query <strong>data.win.eventdata.commandLine:*net* *user*</strong> on the search head, as the query pertained to user account modifications.<br>From the provided screenshots, it is evident that the 'guest' account was activated using the /activate:yes parameter. Subsequently, the command net user guest I_AM_M0NIT0R1NG was executed, setting the 'guest' account password to I_AM_M0NIT0R1NG.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** net.exe is a tool for managing user accounts, network resources, services, and various network settings in Windows.

<!-- wp:image {"id":2282,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-833.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-833.png?w=945" alt="" class="wp-image-2282" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2283,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-834.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-834.png?w=945" alt="" class="wp-image-2283" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the name of the .exe that was used to dump credentials?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Knowing that Mimikatz is a credential dumping tool, I used it in the search head.<br>From the output, I discovered a script executes memotech.exe (originally named Mimikatz.exe) to analyze a memory dump file (lsass.DMP) and extract detailed logon credentials using the sekurlsa plugin commands. After the extraction of credentials, the tool terminates itself.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2286,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-835.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-835.png?w=945" alt="" class="wp-image-2286" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the name of the .exe that was used to dump credentiData was exfiltrated from the host. What was the flag that was part of the data?als?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By searching for the keyword "post" in data.win.eventdata.commandLine, I discovered the only log entry indicating that the script uploads secrets and API keys to Pastebin via its API. The script configures the API key, the content to be posted, and the Pastebin API URL, then sends a POST request with this data.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2287,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-836.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-836.png?w=945" alt="" class="wp-image-2287" /></a></figure>
<!-- /wp:image -->

