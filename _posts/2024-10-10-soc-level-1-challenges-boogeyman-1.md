---
layout: post
title: SOC Level 1 Challenges – Boogeyman 1
date: 2024-10-10 11:10
author: yurenjoeysie
comments: true
categories: [Command and Control, CyberChef, Endpoint, Endpoint Monitoring, Endpoint Security, Event ID, Incident Response, Indicators of Compromise, Linux, Linux Forensics, Network Forensics, Network Security, Phishing, PhishTool, PowerShell, Python, Threat Intelligence, Traffic Analysis, TryHackMe Challenge Rooms, TShark, VirusTotal, Windows Event Logs, Wireshark]
---
<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size">Click <a href="https://tryhackme.com/r/room/boogeyman1">here</a> to enter the challenge room on Try Hack Me</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Artefacts</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">For the investigation, we will be provided with the following artefacts:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Copy of the phishing email (`dump.eml`)</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">PowerShell logs from Julianne's workstation (`powershell.json`)</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Packet capture from the same workstation (`capture.pcapng`)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The `powershell.json` file contains JSON-formatted PowerShell logs extracted from its original EVTX file using the evtx2json tool.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Tools</h1>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Thunderbird: A free and open-source cross-platform email client.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">LNKParse3: A Python package for analyzing binary files with LNK extensions.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Wireshark: A GUI-based packet analyzer.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Tshark: The CLI version of Wireshark.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">jq: A lightweight and flexible command-line JSON processor.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">[Email Analysis] Look at that headers!</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Scenario</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Julianne, a finance employee at Quick Logistics LLC, received a follow-up email about an unpaid invoice from their business partner, B Packaging Inc. Unbeknownst to her, the attached document was malicious and compromised her workstation.<br><br>The security team flagged the suspicious execution of the attachment, alongside phishing reports from other finance department employees, indicating a targeted attack on the finance team. Recent trends show that the initial TTP used for the malicious attachment is linked to a new threat group called Boogeyman, known for targeting the logistics sector.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Given the initial information, we know the compromise began with a phishing email. Let's start by analyzing the dump.eml file located in the artefacts directory. There are two methods to analyze the headers and rebuild the attachment:<br><br><strong>Manual Method</strong>: Use command-line tools such as cat, grep, base64, and sed. You can manually analyze the contents and build the attachment by decoding the string located at the bottom of the file.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"fontSize":"small"} -->
<ul class="wp-block-list has-small-font-size"><!-- wp:list-item -->
<li>$ echo # sample command to rebuild the payload, presuming the encoded payload is written in another file, without all line terminators</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>$ cat *PAYLOAD FILE* | base64 -d &gt; Invoice.zip</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Automated Method</strong>: Double-click the EML file to open it in Thunderbird. From there, you can save and extract the attachment. Once the payload from the encrypted archive is extracted, use LNKParse3 to analyze the information contained within the payload.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">$ lnkparse *LNK FILE*</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline">What is the email address used to send the phishing email?</span></strong><br>We can locate the source email address in the email header in Thunderbird.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3277,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-272.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-272.png?w=1024" alt="" class="wp-image-3277" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u>What is the email address of the victim?</u></strong></span></strong><br>The victim's email address is available in the screenshot above.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u>What is the name of the third-party mail relay service used by the attacker based on the DKIM-Signature and List-Unsubscribe headers?</u></strong></span></strong><br>We can identify the email relay service provider in the DKIM-Signature within the source code.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3278,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-273.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-273.png?w=1024" alt="" class="wp-image-3278" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The 'd=' tag specifies the domain that is responsible for signing the email. This domain is typically the organization or entity that sent the email, and it is used to verify the authenticity of the email's sender.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u>What is the name of the file inside the encrypted attachment?</u></strong></span></strong><br>After saving and extracting the attachment from the .zip file, we can see the filename.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: Ensure not to open the file, as we cannot confirm if it is safe.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u>What is the password of the encrypted attachment?</u></strong></span></strong><br>We can find the password in the email's content.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3279,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-274.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-274.png?w=1024" alt="" class="wp-image-3279" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong>Based on the result of the lnkparse tool, what is the encoded payload found in the Command Line Arguments field?</strong></span></strong><br>By using the lnkparse tool and grep keyword "Command", we can find the encoded value in the CLI arguments.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ lnkparse Invoice_20230103.lnk | grep -e "Command"</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3280,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-275.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-275.png?w=1024" alt="" class="wp-image-3280" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">[Endpoint Security] Are you sure that’s an invoice?</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Scenario</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the initial findings, we discovered how the malicious attachment compromised Julianne's workstation:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">A PowerShell command was executed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Decoding the payload reveals the starting point of endpoint activities.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">With the following discoveries, we should now proceed to analyze the PowerShell logs to uncover the potential impact of the attack:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Start by searching for the execution of the initial payload in the PowerShell logs based on the previous findings.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Since the data is in JSON format, use the `jq` command in the CLI to parse it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Be aware that some logs may be redundant and lack critical information; these can be ignored.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">JQ Cheatsheet</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">jq is a command-line tool for processing JSON data. It works well with other text-processing commands. Refer to the table below for guidance on parsing logs for this task. For more details, visit <a href="https://jqlang.github.io/jq/manual/">jq 1.7 Manual</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:table {"backgroundColor":"tertiary","fontSize":"small"} -->
<figure class="wp-block-table has-small-font-size"><table class="has-tertiary-background-color has-background has-fixed-layout"><tbody><tr><td>Parse all&nbsp;JSON&nbsp;into beautified output</td><td>cat powershell.json | jq</td></tr><tr><td>Print all values from a specific field without printing the field</td><td>cat powershell.json | jq '.Field1'</td></tr><tr><td>Print all values from a specific field</td><td>cat powershell.json | jq '{Field1}'</td></tr><tr><td>Print values from multiple fields</td><td>cat powershell.json | jq '{Field1,&nbsp;Field2}'</td></tr><tr><td>Sort logs based on their Timestamp</td><td>cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]'</td></tr><tr><td>Sort logs based on their Timestamp and print multiple field values</td><td>cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[] | {Field}'</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u>What are the domains used by the attacker for file hosting and C2? Provide the domains in alphabetical order. (e.g. a.domain.com,b.domain.com)</u></strong></span></strong><br>After learning the field names using jq command, we will investigate into ScriptBlockText field.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Since we already identified one of the attacker's domains from previous tasks, we can use the commands sort, uniq, and grep with the keyword "bpakcaging.xyz" to find two domains in the output.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3281,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-276.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-276.png?w=1024" alt="" class="wp-image-3281" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u>What is the name of the enumeration tool downloaded by the attacker?</u></strong></span></strong><br>We know that the ScriptBlockText field contains the PowerShell commands that were executed on the machine, so we will review those commands.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | sort | uniq</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After some examination and research, I discovered the tool used for enumeration. Refer to <a href="https://github.com/GhostPack/Seatbelt?tab=readme-ov-file#command-line-usage">Seatbelt</a> on GitHub for more details about the tool.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3283,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-277.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-277.png?w=1024" alt="" class="wp-image-3283" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u><strong><u>What is the file accessed by the attacker using the downloaded sq3.exe binary? Provide the full file path with escaped backslashes.</u></strong></u></strong></span></strong><br>First, we will trace the binary mentioned in the question using the grep keyword 'sq3.exe'. We found that the command was executed from '.\Music\sq3.exe', so we need to determine the full path.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | grep -e 'sq3.exe'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3285,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-279.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-279.png?w=1024" alt="" class="wp-image-3285" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Next, to find out from which user's workstation the binary was executed, we will trace the attacker's working directory by searching for the command 'cd'. Once we know the username, we need to add the binary's file path to C:\Users\&lt;username&gt; to retrieve the full path.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | grep -e 'cd '</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3286,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-280.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-280.png?w=1024" alt="" class="wp-image-3286" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u><strong>What is the software that uses the file in the previous question?</strong></u></strong></span></strong><br>We can identify the software from the screenshot prior to the last one; it is in the package directory.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u><strong><u>What is the name of the exfiltrated file?</u></strong></u></strong></span></strong><br>After some examination and research in the ScriptBlockText field, I discovered the command that exfiltrate the sensitive file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | sort | uniq</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3289,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-281.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-281.png?w=1024" alt="" class="wp-image-3289" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>NOTE</strong>: The command assigns the sensitive file to the variable $file and the IP address 167.71.211.113 to the variable $destination. It then reads the entire binary content of the specified file into the $bytes variable, preparing it for further transmission.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong><u><strong><u><strong><u>What type of file uses the .kdbx file extension?</u></strong></u></strong></u></strong></span></strong><br>After some research, I found that the file extension is used by an open-source password manager. For more details, please refer to <a href="https://keepass.info/">here</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><span style="text-decoration: underline">What is the encoding used during the exfiltration attempt of the sensitive file?</span></strong></strong></strong></strong><br>By examining the command executed after initiating the file exfiltration, we can identify the encoding method used.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | sort | uniq | grep -A 2 '&lt;sensitive file&gt;'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3291,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-282.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-282.png?w=1024" alt="" class="wp-image-3291" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong><span style="text-decoration: underline">What is the tool used for exfiltration?</span></strong></strong></strong></strong></strong><br>By examining the command executed after initiating the file exfiltration, we can identify the tool used for  exfiltration in this case.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | sort | uniq | grep -A 3 'protected_data.kdbx'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The commands read the binary content of a sensitive file, convert it into a HEX format, and prepare it for exfiltration. It then repeatedly sends the hex-encoded data as DNS queries using nslookup to a specified destination (167.71.211.113), transferring the sensitive information covertly.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3293,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-283.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-283.png?w=1024" alt="" class="wp-image-3293" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">[Network Traffic Analysis] They got us. Call the bank immediately!</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Scenario</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Based on the PowerShell log investigation, we identified the following:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">The attacker accessed and exfiltrated two sensitive files.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Uncovered the domains and ports involved in the network activity.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Discovered the tool used for exfiltration.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Investigation Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">We can conclude the investigation by analyzing the network traffic from the attack:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Use the identified domains and ports.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Review all commands executed by the attacker, which were logged in the packet capture.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Trace the streams of notable commands found in the PowerShell logs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Based on the logs, we can extract the contents of the exfiltrated data by understanding its encoding.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"},"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}}} -->
<h2 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline">What software is used by the attacker to host its presumed file/payload server?</span></strong><br>In the last task, we identified the C2 server's IP address (167.71.211.113). We will use this IP address as a filter in Wireshark to monitor any network connections between the victim and the server.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Wireshark: ip.addr == 167.71.211.113</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining one of the HTTP packets, we can determine the application used by the server.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3296,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-284.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-284.png?w=1024" alt="" class="wp-image-3296" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I also attempted to use Tshark with the field http.server and filtered for the C2 server's IP address.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r capture.pcapng -T fields -e ip.src -e ip.dst -e http.server | sort -r | uniq | grep -e '167.71.211.113'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3298,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-285.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-285.png?w=1024" alt="" class="wp-image-3298" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: SimpleHTTP is a built-in module in Python 2, known as the "http.server" package in Python 3. It allows users to set up a basic web server to serve files directly from a directory using commands.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><span style="text-decoration: underline">What HTTP method is used by the C2 for the output of the commands executed by the attacker?</span></strong></strong><br>The method can be identified in two ways: first, within the same HTTP stream from the previous task.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3299,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-286.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-286.png?w=1024" alt="" class="wp-image-3299" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">And second, through the command we used to determine the tool used for exfiltration.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | sort | uniq | grep -A 3 'protected_data.kdbx'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3300,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-287.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-287.png?w=1024" alt="" class="wp-image-3300" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong>What is the protocol used during the exfiltration activity?</strong></span></strong><br>We can reuse the same command from the previous question, and the output will indicate the application used for the exfiltration activity. We then just need to determine the protocol used, which is quite clear.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat powershell.json | jq '.ScriptBlockText' | sort | uniq | grep -A 3 'protected_data.kdbx'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3301,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-288.png?w=1024" alt="" class="wp-image-3301" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong>What is the password of the exfiltrated file?</strong></span></strong><br>Based on the hint "The password is stored in the database file accessed by the attacker", we can examine the POST request packets sent over the network.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Wireshark: http.request.method == POST</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To retrieve the data that’s sent over the network, we need to copy the html value from the packet, then decode it from the Decimal. I then found the data that we are looking for in the packet No.44467.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3302,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-289.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-289.png?w=1024" alt="" class="wp-image-3302" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: Some network protocols and file formats define their data in decimal. I.e. HTTP headers, JSON objects, and CLI tools may expect numerical values in decimal. This adherence to common practices minimizes potential misinterpretations during data transmission.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After decoding by CyberChef, I found the password of the file.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3303,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-290.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-290.png?w=1024" alt="" class="wp-image-3303" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><span style="text-decoration: underline"><strong>What is the credit card number stored inside the exfiltrated file?</strong></span></strong><br>Remember that the attacker attempted to use the DNS protocol to transfer the data. We will extract DNS queries using Tshark. The output shows some HEX values preceding the domains, and these HEX values are the same for both the domains and their subdomains.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r capture.pcapng -T fields -e dns.qry.name | awk NF | sort | uniq</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3305,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-291.png?w=1024" alt="" class="wp-image-3305" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To only capture the HEX value, we will filter for those containing "bpakcaging.xyz," isolates the portion before the first dot using cut, and removes duplicates with uniq. Then save it in to a file for modification.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r capture.pcapng -T fields -e dns.qry.name | grep &lt;domain&gt; | cut -f 1 -d "." | uniq &gt; file.txt</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once we open the text file, we will notice some unnecessary text values, such as "cdn," "files," and others.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3306,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-292.png?w=1024" alt="" class="wp-image-3306" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once the text file contains only HEX values, we will use xxd to convert the HEX input back into binary format and output it as the password manager’s file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ cat file.txt | xxd -r -p &gt; secret.kdbx</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: Since it was previously stated that the attacker successfully transmitted the password manager's file over the network, we can confirm that the file we reconstructed is the one used by the password manager. Therefore, we will use the .kdbx format.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After that, we can open the file using the password retrieved in the previous task. The credit card number can then be found in the file. By right-clicking on "Company Card," selecting "Copy Field," and then "Account Number," we can obtain the value.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3308,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-293.png?w=1024" alt="" class="wp-image-3308" /></figure>
<!-- /wp:image -->
