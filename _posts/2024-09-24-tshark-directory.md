---
layout: post
title: TShark – Directory
subtitle: Inspect engress traffic from compromised machine to malicious domain with TShark
date: 2024-09-24 00:42
author: Ren Sie
comments: true
category: network-traffic-inspection
---

{: .box-success}
 Refer to [Directory](https://tryhackme.com/r/room/tsharkchallengestwo) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">An alert has been triggered: "A user came across a poor file index, and their curiosity led to problems". </p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Inspect the provided <strong>directory-curiosity.pcap</strong> located in ~/Desktop/exercise-files and retrieve the artefacts to confirm that this alert is a true positive.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Investigate the DNS queries. And investigate the domains by using VirusTotal. According to VirusTotal, there is a domain marked as malicious/suspicious.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the name of the malicious/suspicious domain?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Identify all the domains that the victim has communicated with and input them into VirusTotal to determine which ones are suspicious.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">$ <kbd>tshark -r directory-curiosity.pcap -T fields -e dns.qry.name | awk NF | sort | uniq</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1629,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-572.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-572.png?w=1024" alt="" class="wp-image-1629" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1624,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-571.png?w=1024" alt="" class="wp-image-1624" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the total number of HTTP requests sent to the malicious domain?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Search for all HTTP request packets in the .pcap file, and count how many packets are to the malicious domain with uniq -c.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">$ <kbd>tshark -r directory-curiosity.pcap -Y 'http.request' -T fields -e http.host | awk NF | sort | uniq -c | grep -i 'jx2-bavuong'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1631,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-573.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-573.png?w=1024" alt="" class="wp-image-1631" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the IP address associated with the malicious domain?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Since we already know the domain name and the question is asking for the IP address, we will use&nbsp;<code>dns.a</code>&nbsp;and&nbsp;<code>dns.qry.name</code>&nbsp;to retrieve the information.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">-e dns.a: Extracts IPv4 addresses returned in DNS responses.<br>-e dns.qry.name: Extracts domain names that were queried in DNS requests.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r directory-curiosity.pcap -T fields -e dns.a -e dns.qry.name | awk NF | sort | uniq | grep -i 'jx2-bavuong'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1633,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-574.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-574.png?w=1024" alt="" class="wp-image-1633" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the server info of the suspicious domain?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">By following the http conversation, we know the malicious server’s information.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r directory-curiosity.pcap -z follow,http,ascii,0 -q | grep -i 'server'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1635,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-575.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-575.png?w=1024" alt="" class="wp-image-1635" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Follow the "first TCP stream" in "ASCII" and investigate the output. What is the number of listed files?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">By examining the HTML content from the server's response, we can identify that there are three files.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r directory-curiosity.pcap -z follow,tcp,ascii,0 -q</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1637,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-576.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-576.png?w=1024" alt="" class="wp-image-1637" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the filename of the first file?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">The filename of the first file can be found in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Export all HTTP traffic objects, what is the name of the downloaded executable file?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">The name of the downloaded .exe file can be found in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the SHA256 value of the malicious file?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Using the export-objects function to extract the file that’s captured in the .pcap file to the export directory.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>$ tshark -r directory-curiosity.pcap --export-objects http,~/Desktop/export -q<br>$ sha256sum <kbd>\~/Desktop/export</kbd>/vlauto.exe</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1640,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-577.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-577.png?w=1024" alt="" class="wp-image-1640" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Search the SHA256 value of the file on VirusTotal, what is the "PEiD packer" value?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Paste the hash value into <a href="https://www.virustotal.com/gui/file/b4851333efaf399889456f78eac0fd532e9d8791b23a86a19402c1164aed20de/details">VirusTotal</a>, then navigate to the Details tab to find the value of the PEiD packer.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** PEiD packer: .NET executable" indicates the file is a .NET application and is not packed or encrypted with a tool commonly recognized by PEiD.

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Search the SHA256 value of the file on VirtusTotal, what does the "Lastline Sandbox" flag this as?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Continue with the last task by navigating to the <strong>Behavior</strong> tab, where we will find the behavior tags from Lastline Sandbox.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1643,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-578.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-578.png?w=1024" alt="" class="wp-image-1643" /></a></figure>
<!-- /wp:image -->

