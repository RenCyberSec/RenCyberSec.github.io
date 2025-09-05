---
layout: post
title: Windows Forensics Challenge 2.0
date: 2024-11-21 23:44
author: yurenjoeysie
comments: true
categories: [Command and Control, Command Prompt, Endpoint, Endpoint Monitoring, Endpoint Security, Event Viewer, File System, Hash, Incident Response, Indicators of Compromise, Linux, logs, LOKI, PowerShell, Process Explorer, Process Hacker, Ransomware, Registry Explorer, Registry Key, Strings, Task Scheduler, TryHackMe Challenge Rooms, Windows Event Logs, Windows Forensic, Windows Process, Windows System, Yara Rule]
---
<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size">Click&nbsp;<a href="https://tryhackme.com/r/room/investigatingwindows2">here</a>&nbsp;to enter the challenge room on Try Hack Me<br>Click <a href="https://1earnwithren.wordpress.com/2024/11/03/windows-forensics-challenge/">here</a> for the previous challenge Windows Forensics Challenge 1.0</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"elements":{"link":{"color":{"text":"#74103e"}}},"color":{"text":"#74103e"}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Tool</h1>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Task Scheduler: For viewing and analyzing scheduled tasks.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><a href="https://support.microsoft.com/en-us/windows/system-configuration-tools-in-windows-f8a49657-b038-43b8-82d3-28bea0c5666b">Registry Editor</a>: For searching registry keys related to scheduled tasks.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><a href="https://learn.microsoft.com/en-us/sysinternals/">Sysinternals</a> Suite:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><span style="font-size: 1rem;background-color: var(--wp--preset--color--background);color: var(--wp--preset--color--foreground);font-family: var(--wp--preset--font-family--albura)">Autoruns: Identify WMI entries and processes.</span></li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><span style="font-size: 1rem;background-color: var(--wp--preset--color--background);color: var(--wp--preset--color--foreground);font-family: var(--wp--preset--font-family--albura)">Process Explorer: Investigating parent processes and process details.</span></li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><span style="font-size: 1rem;background-color: var(--wp--preset--color--background);color: var(--wp--preset--color--foreground);font-family: var(--wp--preset--font-family--albura)">Process Monitor: Capturing and analyzing process operations.</span></li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><span style="font-size: 1rem;background-color: var(--wp--preset--color--background);color: var(--wp--preset--color--foreground);font-family: var(--wp--preset--font-family--albura)">Strings.exe: For extracting strings from binaries.</span></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><a href="https://github.com/Neo23x0/Loki">Loki</a>: IOC (Indicator of Compromise) scanner.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><a href="https://github.com/PKRoma/ProcessHacker">Process Hacker</a>: Inspecting disk operations and identifying unusual processes.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size"><a href="https://virustotal.github.io/yara/">Yara</a>: Creating and running custom rules to detect suspicious binaries and files.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#1 Which registry key matches the command in the scheduled task?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Open the "Task Scheduler" to view the scheduled tasks. Then, check each task's command under the Actions tab.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3685,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-21.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-21.png?w=894" alt="" class="wp-image-3685" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Open "Registry Editor" and search (CTRL+F) for the command keyword from each scheduled task. One matching registry key will appear.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3686,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-22.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-22.png?w=1024" alt="" class="wp-image-3686" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#2 Which analysis tool closes immediately when launched?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The available tools are Sysinternals tools on the machine. Since there are many options, we shouldn't open each one.<br>Open Desktop\Tools\SysinternalsSuite\Autoruns.exe to find WMI entries. One, called "KillProcess," likely stops processes, helping us identify which binary is affected.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3688,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-23.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-23.png?w=1024" alt="" class="wp-image-3688" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#3 What is the full WQL Query associated with this script?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>:&nbsp; WQL is a language used to query WMI for system management and monitoring.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#4 What is the script language?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#5 What is the name of the other script?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#6 What is the software company name found in the script?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Double-click the other script to find the software company in the source code.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3690,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-24.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-24.png?w=1024" alt="" class="wp-image-3690" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#7 What websites are associated with this company?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Use the search function (CTRL+F) for "http://" to find URLs linked to the company.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3691,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-25.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-25.png?w=819" alt="" class="wp-image-3691" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":3692,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-26.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-26.png?w=819" alt="" class="wp-image-3692" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#8 Search online for the script name from Q5 from the previous answer. What attack script appears in the results?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Search for the script name online, and we will find a <a href="https://github.com/mattifestation/WMI_Backdoor/blob/master/WMIBackdoor.ps1">GitHub</a> page with the script name.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3695,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-27.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-27.png?w=1024" alt="" class="wp-image-3695" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#9 What is the location of this file within the local machine?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Search for the file name in Windows Explorer to find the full path.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3696,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-28.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-28.png?w=983" alt="" class="wp-image-3696" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#10 Which processes open and close every few minutes?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Refer to the screenshot from question #1 to see two scheduled tasks running periodically. The executable name is in the Actions tab.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#11 What is the parent process for these 2 processes?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To learn more about the processes, open Process Explorer. Since we identified the script that terminates the executable, we can delete or disable it. Press the space bar to pause process capture and easily spot the parent process for the two processes.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3698,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-29.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-29.png?w=545" alt="" class="wp-image-3698" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#12 What is the first operation for the first of the 2 processes?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To capture the first operation, open Process Monitor and add the process name to the filter. When the target process runs, Procmon will capture it, and we can retrieve the operation.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3699,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-30.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-30.png?w=1024" alt="" class="wp-image-3699" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#13 Inspect the properties of the first occurrence of the process. In the Event tab, what are the four pieces of information displayed?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Open the properties of the first occurrence to find the required information.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3700,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-31.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-31.png?w=540" alt="" class="wp-image-3700" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#14 What is the name of the unusual process in the disk operations?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After opening the Disk tab in Process Hacker, we will see some unusual process names.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3701,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-32.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-32.png?w=1024" alt="" class="wp-image-3701" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#15 Run Loki and check the output. What is the module name after `Init`?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After running Loki for 5-10 minutes, logs will be generated in the same directory.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3702,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-33.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-33.png?w=953" alt="" class="wp-image-3702" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Open one of the logs to find the next module name after Init.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3703,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-34.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-34.png?w=913" alt="" class="wp-image-3703" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#16 Regarding the 2nd warning, what is the name of the eventFilter?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The process output can be large as it scans the entire disk. We can redirect the result to a text file. And we will use this text file for the following questions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>C:\Users\Desktop\Tools\loki_0.33.0\loki &gt; loki.exe &gt; OUTPUT.txt</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Search for keyword "WARNING" in the output file to find the second warning and its name.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3705,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-35.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-35.png?w=1024" alt="" class="wp-image-3705" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#17 For the 4th warning, what is the class name?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#18 What binary alert has the following 4d5a90000300000004000000ffff0000b8000000 as FIRST_BYTES?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By searching for the keyword, we can identify the binary that caused the alert.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3706,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-36.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-36.png?w=1024" alt="" class="wp-image-3706" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The first_bytes indicate the signature of the file, specifically the MZ header. Loki highlights the first bytes because they help identify suspicious executable files for efficient threat detection.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#19 According to the results, what is the description listed for reason 1?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the DESC in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#20 Which binary alert is marked as APT Cloaked?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Search for the keyword "APT" to find the binary that triggered the alert.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3708,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-37.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-37.png?w=1024" alt="" class="wp-image-3708" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#21 What are the matches?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#22 Which binary alert is associated with somethingwindows.dmp found in C:\TMP?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Search for the keyword "somethingwindows" to find the binary that triggered the alert.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3709,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-38.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-38.png?w=1024" alt="" class="wp-image-3709" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#23 Which binary is encrypted that is like a trojan?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Search for the keyword "encrypt" to find the binary.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3710,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-39.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-39.png?w=1024" alt="" class="wp-image-3710" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: Derusbi is a backdoor Trojan. It encrypts its executable code using a XOR operation with a 4-byte key to evade signature-based detection and prevent analysis by security tools.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#24 What is the binary that can masquerade itself as a legitimate core Windows process/image.</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After question #27, we know the directory to find the binary masked as a legitimate Windows process.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3711,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-40.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-40.png?w=719" alt="" class="wp-image-3711" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#25 What is the full path location for the legitimate version?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After researching online, we can find its full path.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3712,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-41.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-41.png?w=985" alt="" class="wp-image-3712" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#26 What is the description listed for reason 1?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Since we know the malicious binary name, we can use it as a keyword to search the output file.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3713,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-42.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-42.png?w=1024" alt="" class="wp-image-3713" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#27 What is the file in the same location that is labeled as a hacktool?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Search for "hacktool" to find the binary. Use the path to answer question #24.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3714,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-43.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-43.png?w=1024" alt="" class="wp-image-3714" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#28 What is the name of the Yara Rule MATCH?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#29 Which binary didn't show in the Loki results?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">I found the directory (C:\TMP) with several suspicious binaries flagged by Loki. After searching for the binary name in the Loki output, the missed one is identified.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3715,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-44.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-44.png?w=891" alt="" class="wp-image-3715" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}},"typography":{"fontSize":"1.5rem"}}} -->
<h1 class="wp-block-heading has-text-color has-link-color" style="color:#74103e;font-size:1.5rem">#30 Complete the YAR rule file in the Tools folder on the Desktop. What are the 3 strings needed to detect the binary Loki missed?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After running the Yara against the /TMP, but it only pickup the dmp file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>C:\Users\Desktop\Tools\yara &gt; yara64.exe test.yar C:\TMP</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3716,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-45.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-45.png?w=1024" alt="" class="wp-image-3716" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Open the yara file with text editor, we can learn there are strings missing in one of the rules.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3717,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-46.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-46.png?w=1024" alt="" class="wp-image-3717" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will use SysinternalsSuite to extract the strings and complete the missing rule. The file extension "??1" is likely ".ps1," and "?x?" is likely ".exe." The 3rd string appears to be a version, so I started with "v1" and found with "v2."</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>C:\Users\Desktop\SysinternalsSuite &gt; strings.exe C:\TMP\mim.exe | findstr /C:".ps1" /C:".exe" /C:"v2."</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":3718,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/11/image-47.png?w=1024" alt="" class="wp-image-3718" /></figure>
<!-- /wp:image -->
