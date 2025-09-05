---
layout: post
title: TShark - Teamwork
subtitle: Inspect engress suspicious traffic to the malicious domain with TShark
date: 2024-09-24 00:11
author: Ren Sie
comments: true
category: network-traffic-inspection
---

{: .box-success}
 Refer to [Teamwork](https://tryhackme.com/r/room/tsharkchallengesone) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">An alert has been triggered: "The threat research team discovered a <strong>suspicious domain</strong> that could pose a potential threat to the organization."</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Review the <strong>teamwork.pcap</strong> file located in ~/Desktop/exercise-files and create artifacts for detection tools. According to VirusTotal, there is a domain marked as malicious/suspicious.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Investigate the contacted domains.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Investigate the domains by using VirusTotal.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the full URL of the malicious/suspicious domain address?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Investigate the HTTP request and response records to identify the available domain name in this packet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">$ <kbd>tshark -r teamwork.pcap -z http_seq,tree -q</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">We know this is a malicious/suspicious domain address because of:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Weird Domain: It’s pretending to be PayPal but has suspicious subdomain.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Strange Paths: URLs /<strong>suspecious.php</strong>, which are unusual.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":1595,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-563.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-563.png?w=1024" alt="" class="wp-image-1595" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>When was the URL of the suspicious domain address first submitted to VirusTotal?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By pasting the FQDN of the malicious or suspicious domain into VirusTotal, we can find out the first submission date.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1597,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-564.png?w=709" alt="" class="wp-image-1597" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1600,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-566.png?w=709" alt="" class="wp-image-1600" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Which known service was the domain trying to impersonate?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">It is obvious that the suspicious domain was pretending to be <strong>PayPal</strong>, and it has suspicious subdomain.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the IP address of the malicious domain?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Since we already know the domain name and the question is asking for the IP address, we will use <code>dns.a</code> and <code>dns.qry.name</code> to retrieve the information.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** -e dns.a: Extracts IPv4 addresses returned in DNS responses.<br>-e dns.qry.name: Extracts domain names that were queried in DNS requests.

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r teamwork.pcap -T fields -e dns.a -e dns.qry.name | awk NF | sort | uniq | grep -i 'paypal'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1607,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-568.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-568.png?w=1024" alt="" class="wp-image-1607" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the email address that was used?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Firstly, find out which http traffic that contains keyword “mail".</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r teamwork.pcap -Y 'http contains "mail"'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1609,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-569.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-569.png?w=1024" alt="" class="wp-image-1609" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">From the output, we know that the email can be found in one of the POST request packets, so we need to investigate all the POST request packets.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r teamwork.pcap -Y 'http.request.method == POST' -T fields -e http.file_data</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1611,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-570.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-570.png?w=1024" alt="" class="wp-image-1611" /></a></figure>
<!-- /wp:image -->


