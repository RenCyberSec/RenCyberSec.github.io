---
layout: post
title: SOC Level 1 Challenges - Tempest
date: 2024-10-08 01:05
author: yurenjoeysie
comments: true
categories: [Brim, Command and Control, CyberChef, Endpoint Monitoring, Endpoint Security, Event ID, Event Viewer, EvtxEcmd, Hash, Incident Response, Indicators of Compromise, Network Security, PowerShell, Sysmon, SysmonView, Threat Intelligence, Timeline Explorer, Traffic Analysis, TryHackMe Challenge Rooms, VirusTotal, Windows Event Logs, Windows Forensic, Windows Process, Windows System, Wireshark]
---
<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size">Click <a href="https://tryhackme.com/r/room/tempestincident">here</a> to enter the challenge room on Try Hack Me</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Preparation - Tools and Artifacts</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Toolset</h2>
<!-- /wp:heading -->

<!-- wp:list {"fontSize":"small"} -->
<ul class="wp-block-list has-small-font-size"><!-- wp:list-item -->
<li>Sysmon Logs</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Windows Event Logs</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Packet Capture</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Endpoint Logs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To analyze Windows artefacts like Windows Event Logs and Sysmon logs, we will use the following tools:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">EvtxEcmd</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Timeline Explorer</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">SysmonView</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Event Viewer</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Endpoint Logs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To analyze the provided packet capture, we will use the following tools:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Wireshark</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Brim</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Initial Access - Malicious Document</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Tempest Incident</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In this incident, we will serve as an Incident Responder following an alert triaged by a SOC analyst. The analyst has confirmed that the alert is of CRITICAL severity and requires further investigation. According to the SOC analyst, the intrusion began with a malicious document. The key details from the alert are as follows:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">The malicious document has a .doc extension.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">The user downloaded the document via chrome.exe.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">The document executed a chain of commands to achieve code execution.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To assist with the investigation, refer to the team's cheatsheet for this scenario:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Begin with the events generated by Sysmon.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Use EvtxEcmd, Timeline Explorer, and SysmonView to interpret Sysmon logs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Follow the child processes of WinWord.exe.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Apply filters like ParentProcessID or ProcessID to correlate process relationships.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Focus on Sysmon events such as Process Creation (Event ID 1) and DNS Queries (Event ID 22) to analyze the activity from the malicious document.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Before investigating with <strong>Timeline Explorer</strong>, we need to convert the Sysmon logs to a .csv file. After that, we will load the `sysmon.csv` file into Timeline Explorer.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":null,"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>C:\Tools\EvtxECmd&gt; EvtxECmd.exe -f "&lt;Path_sysmon.evtx&gt;" --csv '&lt;output_directory&gt;' --csvf &lt; output_file.csv&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Before investigating with <strong>SysmonView</strong>, we need to convert the Sysmon logs to a .xml file. To do this, double-click the `sysmon.evtx` file and select "Save All Events As" from the Action panel. After that, we will load the `sysmon.xml` file into SysmonView.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The user of this machine was compromised by a malicious document. What is the file name of the document?</strong><br>In this scenario, we know that the victim downloaded the malicious file using `chrome.exe`. We can locate the malicious file using both tools.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In SysmonView, search for `chrome.exe` and open all available sessions. The file name will be listed in the summary of the events.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3199,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-232.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-232.png?w=709" alt="" class="wp-image-3199" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In Timeline Explorer, search for "chrome.exe" to find the file name in the Payload Data4 column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3200,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-233.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-233.png?w=1024" alt="" class="wp-image-3200" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the name of the compromised user and machine?</strong></strong><br>In SysmonView, refer to the previous screenshot to identify the username associated with the download of the malicious file in their download directory. The machine name can be found in the tree map.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3201,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-234.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-234.png?w=1024" alt="" class="wp-image-3201" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In Timeline Explorer, we can find the machine name and username in the Username column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3202,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-235.png?w=502" alt="" class="wp-image-3202" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the PID of the Microsoft Word process that opened the malicious document?</strong></strong><br>In SysmonView, search for `WinWord.exe` as indicated in the Investigation Guide. After loading all available sessions, look for a File Created event associated with the executable. By double-clicking it, you can retrieve more details, including the PID.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3203,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-236.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-236.png?w=1024" alt="" class="wp-image-3203" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In Timeline Explorer, search for `WinWord.exe` as suggested in the Investigation Guide. The PID of the MS Word process can be found in the Payload Data1 column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3204,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-237.png?w=1024" alt="" class="wp-image-3204" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>Based on Sysmon logs, what is the IPv4 address resolved by the malicious domain used in the previous question?</strong></strong><br>In SysmonView, we can observe multiple network connections made by the executable. All connections are through port 443, except for one that uses port 80.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3205,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-238.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-238.png?w=413" alt="" class="wp-image-3205" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To confirm, I checked VirusTotal, which indicated that the IP address had malicious activity.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3207,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-240.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-240.png?w=1024" alt="" class="wp-image-3207" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In Timeline Explorer, we can find the suspicious IP address in the Payload Data6 column. Additionally, the executable is attempting to access phishteam.xyz, which is listed in the Payload Data4 column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3208,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-241.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-241.png?w=1024" alt="" class="wp-image-3208" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the base64 encoded string in the malicious payload executed by the document?</strong></strong><br>Since we have the PID that executed the malicious document, we will search for any processes created by it. To do this, look for the PPID in the Payload Data5 column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3209,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-242.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-242.png?w=1024" alt="" class="wp-image-3209" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We can find the executed command in the Executable Info column. Double-click on the cell to view the cell contents.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3210,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-243.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-243.png?w=1024" alt="" class="wp-image-3210" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the CVE number of the exploit used by the attacker to achieve a remote code execution?</strong></strong><br>By searching for "mpsigstub.exe remote code execution" on Google, I found the article titled "<a href="https://www.secpod.com/blog/critical-alert-microsoft-support-diagnostic-tool-rce-vulnerability-exploited-in-the-wild/">Follina: Microsoft Support Diagnostic Tool RCE Vulnerability Under Active Exploitation</a>". This article details the vulnerability, its exploitation, and the associated CVE number.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Initial Access - Stage 2 execution</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the initial findings, we discovered a stage 2 execution: The document successfully executed an encoded base64 command. Decoding this string reveals the exact command chain executed by the malicious document.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">With these discoveries, we can refer back to the cheatsheet to continue the investigation:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">The Autostart execution indicates explorer.exe as its parent process ID.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Child processes of explorer.exe within the event timeframe may be significant.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Process Creation (Event ID 1) and File Creation (Event ID 11) following the document execution are worth examining.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The malicious execution of the payload wrote a file on the system. What is the full target path of the payload?</strong><br>First, we will decode the base64 string obtained from the previous question. By copying and pasting the string into <a href="https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&amp;input=SkdGd2NEMWJSVzUyYVhKdmJtMWxiblJkT2pwSFpYUkdiMnhrWlhKUVlYUm9LQ2RCY0hCc2FXTmhkR2x2YmtSaGRHRW5LVHRqWkNBaUpHRndjRnhOYVdOeWIzTnZablJjVjJsdVpHOTNjMXhUZEdGeWRDQk5aVzUxWEZCeWIyZHlZVzF6WEZOMFlYSjBkWEFpT3lCcGQzSWdhSFIwY0RvdkwzQm9hWE5vZEdWaGJTNTRlWG92TURKa1kyWXdOeTkxY0dSaGRHVXVlbWx3SUMxdmRYUm1hV3hsSUhWd1pHRjBaUzU2YVhBN0lFVjRjR0Z1WkMxQmNtTm9hWFpsSUM1Y2RYQmtZWFJsTG5wcGNDQXRSR1Z6ZEdsdVlYUnBiMjVRWVhSb0lDNDdJSEp0SUhWd1pHRjBaUzU2YVhBN0NnPT0">Cyberchef</a>, we can determine the payload executed by the malicious document.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3215,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-244.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-244.png?w=1024" alt="" class="wp-image-3215" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The command downloads a .ZIP file and extracts its contents into the Startup folder of the Start Menu. After extraction, it deletes the ZIP file to clean up.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By analyzing the command, I noted that the startup folder is involved. By inputting the path ($app\Microsoft\Windows\Start Menu\Programs\Startup) into ChatGPT, I received the full path. I then just needed to fill in the username.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>C:\Users\&lt;Username&gt;\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>The implanted payload executes once the user logs into the machine. What is the executed command upon a successful login of the compromised user?</strong></strong><br>As the Investigation Guide suggested, "The Autostart execution indicates explorer.exe as its PPID." I retrieved two PIDs from explorer.exe by searching in Payload Data3 column on Timeline Explorer.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3217,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-245.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-245.png?w=1024" alt="" class="wp-image-3217" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I filtered the PIDs separately in Payload Data3 column and found the processes created by PowerShell.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3218,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-246.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-246.png?w=1024" alt="" class="wp-image-3218" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By double-clicking to open the cell contents, I recognized the domain phishteam[.]xyz, which we encountered in the decoded payload. This is also the executed command following a successful login of the compromised user.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3219,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-247.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-247.png?w=1024" alt="" class="wp-image-3219" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The command uses PowerShell to download an executable file from a URL and saves it to the "Downloads" folder in the Public directory of all users while running the PowerShell window in hidden mode. After downloading, it immediately executes the downloaded file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>Based on Sysmon logs, what is the SHA256 hash of the malicious binary downloaded for stage 2 execution?</strong></strong><br>After identifying the name of the downloaded file (first.exe), I used it as a filter in the Executable Info column to retrieve its SHA256 hash value.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3221,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-248.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-248.png?w=1024" alt="" class="wp-image-3221" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>The stage 2 payload downloaded establishes a connection to a c2 server. What is the domain and port used by the attacker?</strong></strong><br>SysmonView offers a clearer view of the network connections made by the malicious file. After searching for the file (first.exe) and opening the sessions, we can identify the C2 server's domain name, IP address, and port.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3222,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-249.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-249.png?w=472" alt="" class="wp-image-3222" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Initial Access - Malicious Document Traffic</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the collected findings, we determined that the attacker fetched the stage 2 payload remotely:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Identified the domain and IP invoked by the malicious document in the Sysmon logs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Identified another domain and IP used by the stage 2 payload, logged from the same data source.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Since we have identified network-related artefacts, we can refer to our cheatsheet focusing on Network Log Analysis:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Brim and Wireshark to investigate the packet capture.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Look for network events related to the harvested domains and IP addresses.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">A sample Brim filter: _path=="http" "&lt;malicious domain&gt;".</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Data Sources:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"fontSize":"small"} -->
<ul class="wp-block-list has-small-font-size"><!-- wp:list-item -->
<li>Packet Capture</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the URL of the malicious payload embedded in the document?</strong></strong><br>At this stage, we know there are two malicious domains: "phishteam.xyz," executed by `WinWord.exe`, and "solvecyber.xyz," executed by `first.exe` whenever a user logs into the machine. I searched both domains since the question didn't specify which document. I found the URL embedded in the malicious `WinWord.exe` by searching for the first malicious domain, "phishteam.xyz."<br><br>By analyzing the traffic, I learned that the payload first retrieves `index.html`, then `update.zip`. This file is extracted to the startup folder, which will retrieve `first.exe` when a user logs in.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>WireShark: http contains "phishteam.xyz"<br>Brim: _path=="http" "phishteam.xyz" | cut ts, host, uri</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3226,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-250.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-250.png?w=1024" alt="" class="wp-image-3226" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the encoding used by the attacker on the c2 connection?</strong></strong><br>I followed the HTTP stream of the packet that retrieves `index.html` from the C2 server on WireShark. By using the search term "encod," I discovered another encoded string.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3227,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-251.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-251.png?w=1024" alt="" class="wp-image-3227" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>The malicious c2 binary sends a payload using a parameter that contains the executed command results. What is the parameter used by the binary?</strong></strong><br>As the question mention the C2 binary (first.exe), we now search for the second malicious domain " solvecyber.xyz".</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>WireShark: http contains "solvecyber.xyz"</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The request URI indicates a resource being requested from the server, with the portion after the ? serving as a query string. The query parameter contains a Base64-encoded value that decodes to the command "whoami - tempe st\benimart".</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3229,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-252.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-252.png?w=1024" alt="" class="wp-image-3229" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>The malicious c2 binary connects to a specific URL to get the command to be executed. What is the URL used by the binary?</strong></strong><br>As the screenshot shows, the specific URL before the query serves as a unique identifier for a specific resource on the server, which used for accessing a particular file, or executing a command.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the HTTP method used by the binary?</strong></strong><br>The HTTP method can be found in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>Based on the user agent, what programming language was used by the attacker to compile the binary?</strong></strong><br>We can identify the programming language by examining the user-agent of the same packet in Wireshark, as mentioned in the previous question.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: Nim is a statically typed language that compiles to C, and its HTTP client library facilitates making HTTP requests.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3231,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-253.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-253.png?w=1024" alt="" class="wp-image-3231" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Discovery - Internal Reconnaissance</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the collected findings, we have discovered that the malicious binary continuously uses C2 traffic:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Decode the encoded string found in the network traffic.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Traffic includes commands and outputs executed by the attacker.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To continue with the investigation, we may focus on the following information:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Find network and process events connecting to the malicious domain.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Find network events that contain an encoded command.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Use Brim to filter all packets containing the encoded string.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Look for endpoint enumeration commands since the attacker is already inside the machine.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In addition, we may refer to cheatsheet for Brim to investigate the encoded traffic, to get all HTTP requests related to the malicious C2 traffic:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Brim: _path=="http" "&lt;replace domain&gt;" id.resp_p==&lt;replace port&gt; | cut ts, host, id.resp_p, uri | sort ts</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Significant Data Sources:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Packet Capture</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Sysmon</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>The attacker was able to discover a sensitive file inside the machine of the user. What is the password discovered on the file?</strong></strong><br>While investigating the traffic between the compromised machine and the malicious domain using the filters provided in the Investigation Guide, I encountered a payload that mentioned the password instead of any sensitive file content being transferred over the network.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Brim: _path=="http" "solvecyber.xyz" id.resp_p==80 | cut ts, host, id.resp_p, uri | sort ts</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The attacker retrieves commands from the server to enable dynamic execution of tasks, allowing for real-time adaptability based on the target's environment. This approach enhances evasion tactics and helps automate malicious operations, making it harder for security tools to detect and flag suspicious behavior.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3235,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-254.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-254.png?w=1024" alt="" class="wp-image-3235" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong>The attacker then enumerated the list of listening ports inside the machine. What is the listening port that could provide a remote shell inside the machine?</strong></strong></strong><br>As I continued decoding the query, I found a netstat output. Further research revealed that the attacker exploited the WinRM (Windows Remote Management) port. For more information, refer to "<a href="https://www.rapid7.com/blog/post/2012/11/08/abusing-windows-remote-management-winrm-with-metasploit/">Abusing Windows Remote Management (WinRM) with Metasploit</a>" on Rapid7.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3237,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-255.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-255.png?w=1024" alt="" class="wp-image-3237" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong>The attacker then established a reverse socks proxy to access the internal services hosted inside the machine. What is the command executed by the attacker to establish the connection?</strong></strong></strong><br>While browsing through the Sysmon log in Timeline Explorer, I noted that the netstat command was executed at 17:16:51. I then identified a binary that connected to the malicious IP address we noted earlier via port 8080, which is commonly used by Metasploit.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3238,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-256.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-256.png?w=1024" alt="" class="wp-image-3238" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong>What is the SHA256 hash of the binary used by the attacker to establish the reverse socks proxy connection?</strong></strong></strong><br>To find the binary's hash value, navigate to the Payload Data3 column in Timeline Explorer.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3239,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-257.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-257.png?w=1024" alt="" class="wp-image-3239" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong>What is the name of the tool used by the attacker based on the SHA256 hash? Provide the answer in lowercase.</strong></strong></strong><br>By pasting the hash value of the binary into <a href="https://www.virustotal.com/gui/file/8a99353662ccae117d2bb22efd8c43d7169060450be413af763e8ad7522d2451">VirusTotal</a>, we can determine its name.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3240,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-258.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-258.png?w=1024" alt="" class="wp-image-3240" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong>The attacker then used the harvested credentials from the machine. Based on the succeeding process after the execution of the socks proxy, what service did the attacker use to authenticate?</strong></strong></strong><br>As the screenshot shows, the wsmprovhost.exe is executed after the reverse shell binary.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: wsmprovhost.exe acts as a helper process for WinRM, enabling it to execute remote management tasks and commands on a target computer. WinRM provide authentication using various methods, including NTLM, Kerberos, Basic Authentication, and CredSSP.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3242,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-259.png?w=1024" alt="" class="wp-image-3242" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Privilege Escalation - Exploiting Privileges</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the collected findings, the attacker established a stable shell through a reverse SOCKS proxy.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">With this information, we can focus on the following network and endpoint events:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Look for events executed after the successful execution of the reverse SOCKS proxy tool.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Investigate potential privilege escalation attempts, as the attacker has established persistent low-privilege access.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Significant Data Sources:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Packet Capture</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Sysmon</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong>After discovering the privileges of the current user, the attacker then downloaded another binary to be used for privilege escalation. What is the name and the SHA256 hash of the binary?</strong></strong></strong></strong><br>From previous tasks, we know that the attacker executed `ch.exe` for a reverse connection. We will continue tracking any events created after the execution of this malicious binary. As shown in the screenshot, the attacker used PowerShell to download another binary, which is what we are looking for.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3245,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-260.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-260.png?w=1024" alt="" class="wp-image-3245" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To learn it’s SHA256 hash value, we need to search for the event of that binary being executed, and correlate to Payload Data3 column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3246,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-261.png?w=1024" alt="" class="wp-image-3246" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong>Based on the SHA256 hash of the binary, what is the name of the tool used?</strong></strong></strong></strong><br>By pasting the hash value of the binary into <a href="https://www.virustotal.com/gui/file/8524fbc0d73e711e69d60c64f1f1b7bef35c986705880643dd4d5e17779e586d">VirusTotal</a>, we can determine its name.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3247,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-262.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-262.png?w=1024" alt="" class="wp-image-3247" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong>The tool exploits a specific privilege owned by the user. What is the name of the privilege?</strong></strong></strong></strong><br>After some researches on <a href="https://www.virustotal.com/gui/file/8524fbc0d73e711e69d60c64f1f1b7bef35c986705880643dd4d5e17779e586d/community">VirusTotal</a>, I found this GitHub <a href="https://github.com/waldo-irc/CVE-2021-21551">repository</a> talking about the exploitation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: SeImpersonatePrivileges refers to a type of privilege in Windows that allows a user or process to impersonate another user. This is important for security and access control, as it enables an application to perform actions on behalf of a user, with the same permissions and rights that the user has.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3248,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-263.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-263.png?w=1024" alt="" class="wp-image-3248" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong>Then, the attacker executed the tool with another binary to establish a c2 connection. What is the name of the binary?</strong></strong></strong></strong><br>Referring to the screenshot that correlates the malicious binary (`spf.exe`) with its SHA256 hash value, we can see that there's a command associated with another binary.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>C:\Users\benimaru\Downloads\spf.exe -c C:\ProgramData\&lt;binary.exe&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The command executes spf.exe with the -c option, configuring it to establish a reverse shell connection to an attacker's machine. The specified binary contains additional resources or configurations needed for this operation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong>The binary connects to a different port from the first c2 connection. What is the port used?</strong></strong></strong></strong><br>First, I noted the execution time of both binaries from the previous question.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3250,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-264.png?w=1024" alt="" class="wp-image-3250" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Next, I navigated to Wireshark to check for any connections established at the time the binaries were executed. By entering the C2 server’s IP address and looking at the timestamps, I identified an additional port opened for the reverse shell connection.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3251,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-265.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-265.png?w=1024" alt="" class="wp-image-3251" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Actions on Objective - Fully-owned Machine</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now that the attacker has gained administrative privileges on the machine, we should identify all persistence techniques used by the attacker. Additionally, look for unusual executions related to the malicious C2 binary used during privilege escalation.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Now, we can use our cheatsheet to investigate events following a successful privilege escalation:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Useful Brim filter to get all HTTP requests related to the malicious C2 traffic</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">The attacker has gained SYSTEM privileges; therefore, the user context for each malicious execution is now associated with NT AUTHORITY\SYSTEM.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">All child events of the new malicious binary used for C2 should be examined.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Brim: _path=="http" "&lt;replace domain&gt;" id.resp_p==&lt;replace port&gt; | cut ts, host, id.resp_p, uri | sort ts</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Significant Data Sources:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Packet Capture</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Sysmon</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Windows Event Logs</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong>Upon achieving SYSTEM access, the attacker then created two users. What are the account names?</strong></strong></strong></strong></strong><br>To create the user on Windows, we will use `net.exe`. After adding the keyword "net" in the Executable Info column on Timeline Explorer and navigating to the timeline after 17:21:34, when the reverse shell connection was established, I found that two new users were created by `net.exe` and `net1.exe`.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3253,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-266.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-266.png?w=1024" alt="" class="wp-image-3253" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>:<br>net.exe is a built-in Windows command-line utility that allows users to manage network resources, user accounts, and services. net1.exe is an alternative version of net.exe used primarily for compatibility in specific Windows environments or older systems.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong><strong>Prior to the successful creation of the accounts, the attacker executed commands that failed in the creation attempt. What is the missing option that made the attempt fail?</strong></strong></strong></strong></strong></strong><br>I scroll up to the earlier process, found that the commands are missing one option which can be found in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3255,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-267.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-267.png?w=1024" alt="" class="wp-image-3255" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong><strong>Based on windows event logs, the accounts were successfully created. What is the event ID that indicates the account creation activity?</strong></strong></strong></strong></strong></strong><br>Refer to the <a href="https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4720">Microsoft Documentation</a> for details on the Event ID that indicates account creation activity.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3256,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-268.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-268.png?w=1024" alt="" class="wp-image-3256" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong><strong>The attacker added one of the accounts in the local administrator's group. What is the command used by the attacker?</strong></strong></strong></strong></strong></strong><br>The command can be found after the creation of the new users under the same column (Executable Info) on Timeline Explorer.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3259,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-270.png?w=1024" alt="" class="wp-image-3259" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong><strong><strong>Based on windows event logs, the account was successfully added to a sensitive group. What is the event ID that indicates the addition to a sensitive local group?</strong></strong></strong></strong></strong></strong></strong><br>Refer to the <a href="https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4732">Microsoft Documentation</a> for details on the Event ID that indicates the addition to a sensitive local group.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3257,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-269.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-269.png?w=1024" alt="" class="wp-image-3257" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong><strong><strong>After the account creation, the attacker executed a technique to establish persistent administrative access. What is the command executed by the attacker to achieve this?</strong></strong></strong></strong></strong></strong></strong><br>After continuing the investigation into the processes following the commands from previous tasks, I found the command that used the malicious binaries we had identified earlier. By analyzing the command, I learned that it was intended for persistence.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3260,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-271.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-271.png?w=1024" alt="" class="wp-image-3260" /></a></figure>
<!-- /wp:image -->
