---
layout: post
title: Phishing Challenge - The Greenholt Phish
date: 2024-10-03 17:08
author: yurenjoeysie
comments: true
categories: [Cyber Threat Intelligence, DMARC, Email, Endpoint Monitoring, Incident Response, Indicators of Compromise, Linux, Malware Analysis, Network Security, Phishing, SPF, Threat Intelligence, TryHackMe Challenge Rooms]
---
<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Click <a href="https://tryhackme.com/r/room/phishingemails5fgjlzxc">here</a> to enter the challenge room on Try Hack Me</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">A Sales Executive at Greenholt PLC received an unexpected email from a customer. He noted that the customer typically does not use generic greetings like "Good day" and that he was not expecting any money to be transferred to his account. Additionally, the email contained an attachment that he had not requested. He forwarded the email to the Security Operations Center (SOC) for further investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Investigate the email sample (challenge.eml) to determine its legitimacy.<br>Open the EML file using Thunderbird. Right-click on the challenge.eml file, select "Open With," and then choose "Other Application." Scroll down to select "Thunderbird Mail" and click "Open."</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the Transfer Reference Number listed in the email's Subject?</strong><br>The Transfer Reference Number can be found in the email content.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2699,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-50.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-50.png?w=1024" alt="" class="wp-image-2699" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Who is the email from?</strong><br>The sender can be found in the email content.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2700,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-51.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-51.png?w=1024" alt="" class="wp-image-2700" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is his email address?</strong><br>The sender's address can be found in the email's header.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2701,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-52.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-52.png?w=1024" alt="" class="wp-image-2701" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What email address will receive a reply to this email?</strong><br>The Reply-To address can be found in the email's header.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the Originating IP?</strong><br>To discover the IP address, we need to view the source. Click on "More", then "View Source"</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2702,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-53.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-53.png?w=709" alt="" class="wp-image-2702" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In the header, the value of X-Originating-Ip is invalid. But I was able to spot the IP address of the server that sent the email to the next server from "Received: from hwsrv-737338.hostwindsdns.com ([xxx.xxx.xx.xxx]:51810 helo=mutawamarine.com)"</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2703,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-54.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-54.png?w=1024" alt="" class="wp-image-2703" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Who is the owner of the Originating IP?</strong><br>To identify the organization that owns the IP address mentioned in the previous question, look up the IP address using a <a href="https://www.whois.com/whois/192.119.71.157">WHOIS</a> service.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2704,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-55.png?w=1024" alt="" class="wp-image-2704" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the SPF record for the Return-Path domain?</strong><br>Since the question mentioned the Return-Path domain (mutawamarine[.]com), we will use <a href="https://dmarcian.com/spf-survey/">SPF Surveyor</a> to look up its SPF record. The result shows that there is one and only one SPF record.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2705,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-56.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-56.png?w=1004" alt="" class="wp-image-2705" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: The record indicates that the domain (mutawamarine[.]com) authorizes mail sent from servers listed in the spf.protection.outlook.com SPF record to send emails on its behalf. The -all directive means that emails sent from any other servers should be rejected.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the DMARC record for the Return-Path domain?</strong><br>To check the DMARC record, we will use the <a href="https://dmarcian.com/domain-checker/">DMARC Domain Checker</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2698,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-49.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-49.png?w=1024" alt="" class="wp-image-2698" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>NOTE</strong>: This record specifies that emails failing DMARC checks should be quarantined. The fo=1 setting indicates that a failure report should be generated if either SPF or DKIM fails.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the name of the attachment?</strong><br>The name of the attachment can be obtained from the content of the mail or search for (CTRL+F) " attachment" in the source.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2697,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-48.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-48.png?w=1024" alt="" class="wp-image-2697" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the SHA256 hash of the file attachment?</strong><br>Return to the original email and save the attachment. Make sure not to open it, as we cannot confirm whether it is malicious.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2694,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-46.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-46.png?w=569" alt="" class="wp-image-2694" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Open the terminal in the directory where the file is saved and run the following command to retrieve the SHA256 hash value.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sha256 &lt;filename&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2695,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-47.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-47.png?w=1024" alt="" class="wp-image-2695" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the attachments file size?</strong><br>Once we have the hash value, enter it into <a href="https://www.virustotal.com/gui/file/2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f">VirusTotal</a>. We will be able to get the results from there.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2693,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-45.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-45.png?w=1024" alt="" class="wp-image-2693" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the actual file extension of the attachment?</strong><br>The file extension is displayed in the previous screenshot.</p>
<!-- /wp:paragraph -->
