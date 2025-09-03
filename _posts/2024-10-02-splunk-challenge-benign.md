---
layout: post
title: Splunk Challenge - Benign
date: 2024-10-02 20:07
author: yurenjoeysie
comments: true
categories: [Endpoint, Endpoint Monitoring, Endpoint Security, Incident Response, Indicators of Compromise, Network Security, SIEM, TryHackMe Challenge Rooms, Windows Event Logs, Windows Forensic, Windows Process, Windows System]
---
<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Click <a href="https://tryhackme.com/r/room/benign">here</a> to enter the challenge room on Try Hack Me</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">One of the client’s Intrusion Detection Systems (IDS) flagged a suspicious process on a computer in the HR department, suggesting it might be compromised. We observed tools related to network information gathering and scheduled tasks running on the affected machine, which confirmed our suspicion. Because of limited resources, we could only collect the process execution logs with Event ID: 4688. We then imported these logs into Splunk using the win_eventlogs index for further investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Network Information</h2>
<!-- /wp:heading -->

<!-- wp:table {"backgroundColor":"tertiary","fontSize":"small"} -->
<figure class="wp-block-table has-small-font-size"><table class="has-tertiary-background-color has-background has-fixed-layout"><tbody><tr><td><strong>IT Department</strong></td><td><strong>HR department</strong></td><td><strong>Marketing department</strong></td></tr><tr><td>James</td><td>Haroon</td><td>Bell</td></tr><tr><td>Moin</td><td>Chris</td><td>Amelia</td></tr><tr><td>Katrina</td><td>Diana</td><td>Deepak</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Activity</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>How many logs are ingested from the month of March 2022?</strong><br>In accordance with the scenario, the logs are available in the ‘win_eventlogs’ index. Configure the date range starting from March 1, 2022.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index="win_eventlogs"</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2588,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-33.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-33.png?w=1024" alt="" class="wp-image-2588" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?</strong><br>By examining the list of all available usernames, we can identify the imposter account.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index="win_eventlogs"<br>| dedup UserName<br>| table UserName</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2586,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-32.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-32.png?w=859" alt="" class="wp-image-2586" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Which user from the HR department was observed to be running scheduled tasks?</strong><br>By searching for schtasks.exe on the search head, we can identify the executed scheduled tasks. Based on the results, we can identify the user by referencing the department personnel list provided at the beginning.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index="win_eventlogs" schtasks.exe<br>| dedup UserName<br>| table UserName ProcessName</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size">NOTE: schtasks.exe is a Windows utility for creating, deleting, configuring, and displaying scheduled tasks."<br>NOTE: dedup can eliminate duplicate user entries.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2584,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-31.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-31.png?w=1024" alt="" class="wp-image-2584" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.</strong><br>By filtering the events using the HR personnel's' username, I identified that a user used the certutil utility to download benign.exe from a file-sharing host.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":null,"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>index="win_eventlogs" UserName="Chris.fort" OR UserName="haroon" OR UserName="Daina"<br>| dedup ProcessName CommandLine<br>| table UserName ProcessName CommandLine</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2590,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-34.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-34.png?w=1024" alt="" class="wp-image-2590" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size">NOTE: option <code>-urlcache</code> allows Certutil to interact with the URL cache.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>To bypass the security controls, which system process (<a href="https://lolbas-project.github.io/">lolbin</a>) was used to download a payload from the internet?</strong><br>The process name is available in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What was the date that this binary was executed by the infected host?</strong><br>By left-clicking the event and select 'View Events,' we can locate the execution date.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2592,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-35.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-35.png?w=986" alt="" class="wp-image-2592" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Which third-party site was accessed to download the malicious payload?</strong><br>The host site is identified in the CommandLine from the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><strong>NOTE:</strong> The specific host site provides file hosting and sharing services that are commonly used for uploading and distributing files.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?</strong><br>The downloaded file is in the CommandLine as indicated in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?</strong><br>By navigating to the URL found in the CommandLine section of the previous screenshot, we can access the content that was downloaded by the user.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2595,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-36.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-36.png?w=677" alt="" class="wp-image-2595" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the URL that the infected host connected to?</strong><br>The URL is detailed in the CommandLine section of the event log screenshot.</p>
<!-- /wp:paragraph -->
