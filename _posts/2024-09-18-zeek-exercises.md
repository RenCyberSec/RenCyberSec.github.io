---
layout: post
title: Zeek
subtitle: Inspect network traffic in three real-life incident cases: Suspicious DNS Query, Phishing Campaign, and Log4j with Zeek.
date: 2024-09-18 20:32
author: Ren Sie
comments: true
category: network-traffic-inspection
---

{: .box-success}
 Refer to [Zeek Exercises](https://tryhackme.com/r/room/zeekbroexercises) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Case #1 - Anomalous DNS </h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Scenario</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">An alert triggered: "Anomalous DNS Activity". Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">During this exercise, we will use the Zeek command with the -Cr option to analyze the packet capture file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>zeek -Cr dns-tunneling.pcap</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the dns-tunneling.pcap file. Investigate the dns.log file. What is the number of DNS records linked to the IPv6 address?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After reviewing the dns.log, we should focus on the qtype_name field for further investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">MX (Mail Exchange): Directs email to the mail servers responsible for receiving emails for a domain.<br>CNAME (Canonical Name): Maps an alias domain name to a canonical (true) domain name.<br>TXT (Text): Holds arbitrary text data, often used for domain verification and security purposes.<br>AAAA (IPv6): Provides the IPv6 address associated with a domain name.<br>A (IPv4): Provides the IPv4 address associated with a domain name.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1061,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-344.png?w=886" alt="" class="wp-image-1061" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1062,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-345.png?w=886" alt="" class="wp-image-1062" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Use the command grep 'AAAA' to find out how many queries are for AAAA (IPv6) records. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1064,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-346.png?w=886" alt="" class="wp-image-1064" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the conn.log file. What is the longest connection duration?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After reviewing the conn.log, we should focus on duration field for further investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1066,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-347.png?w=886" alt="" class="wp-image-1066" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1067,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-348.png?w=886" alt="" class="wp-image-1067" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the dns.log file. Filter all unique DNS queries. What is the number of unique domain queries?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After reviewing the dns.log, we should focus on the query field for further investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1069,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-349.png?w=886" alt="" class="wp-image-1069" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><strong>rev</strong>: Reverses each domain name. E.g., cisco-update.com &gt; moc.etadpu-ocsic.<br><strong>cut -d '.' -f 1-2</strong>: Splits the reversed name by dots (.) and extract the first two fields. (E.g., moc.etadpu-ocsic).<br><strong>2<sup>nd</sup> rev</strong>: Reverses these parts back to their original order. This gives you the last two segments of the original domain name.<br><strong>sort</strong>: Sorts the result alphabetically.<br><strong>uniq</strong>: Removes duplicate from the list.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1072,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-350.png?w=886" alt="" class="wp-image-1072" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>There is a massive amount of DNS queries sent to the same domain. This is abnormal. Let's find out which hosts are involved in this activity. Investigate the conn.log file. What is the IP address of the source host?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After reviewing the dns.log, we should focus on the id.orig_h field for further investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1075,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-351.png?w=886" alt="" class="wp-image-1075" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">uniq -c: Counts the number of occurrences of each unique output.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1076,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-352.png?w=886" alt="" class="wp-image-1076" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Case #2 - Phishing</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Scenario</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">An alert triggered: "Phishing Attempt".  Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">During this exercise, we will use the Zeek command with the -Cr option to analyze the packet capture file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>zeek -Cr phishing.pcap</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the logs. What is the suspicious source address? </strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining the conn.log, we can identify which IP addresses appear most frequently in the sender field.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** The more frequently an IP address appears in the sender field, the more likely it is to be associated with spam or phishing activities.

<!-- wp:image {"id":1041,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-334.png?w=886" alt="" class="wp-image-1041" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1042,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-335.png?w=886" alt="" class="wp-image-1042" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the http.log file. Which domain address were the malicious files downloaded from?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By analyzing the http.log, we can determine which files are downloaded and from which domain.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">Application/x-dosexec is a MIME type that often indicates executable file, which can be used to distribute malware.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1044,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-336.png?w=886" alt="" class="wp-image-1044" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1045,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-337.png?w=886" alt="" class="wp-image-1045" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the malicious document in VirusTotal. What kind of file is associated with the malicious document?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Using the hash-demo.zeek script, we can obtain the MD5 hash values of files, which we will then upload to VirusTotal for analysis.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>zeek -Cr phishing.pcap hash-demo.zeek.</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By reviewing the files.log, we can identify any malicious files and their corresponding hash values.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1048,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-338.png?w=886" alt="" class="wp-image-1048" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1049,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-339.png?w=886" alt="" class="wp-image-1049" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Upload its hash value to <a href="https://www.virustotal.com/gui/home/upload">VirusTotal</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1050,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-340.png?w=886" alt="" class="wp-image-1050" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Unfortunately, the information which contained malicious file’s file type was removed by VirusTotal by the time I worked on this lab in August 2024. No related information can be found online either.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the extracted malicious .exe file. What is the given file name in Virustotal?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The answer is provided by the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the malicious .exe file in VirusTotal. What is the contacted domain name? Enter your answer in defanged format.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The malicious activity can be found under “Behaviour” tab.<br>To identify the contacted domain, we will go to the "DNS Resolutions" section.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In a fully qualified domain name (FQDN) like dunlop.hopo.org:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"fontSize":"small"} -->
<ul class="wp-block-list has-small-font-size"><!-- wp:list-item -->
<li>"hopo.org" is the main domain name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>"dunlop" is a subdomain of hopo.org.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":1055,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-341.png?w=709" alt="" class="wp-image-1055" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the http.log file. What is the request name of the downloaded malicious .exe file?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining the http.log, we can find the names of downloaded malicious files in the URI field.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1056,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-342.png?w=886" alt="" class="wp-image-1056" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1057,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-343.png?w=886" alt="" class="wp-image-1057" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Case #3 - Log4J</h1>
<!-- /wp:heading -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Scenario</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">An alert triggered: "Log4J Exploitation Attempt". Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Task</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">During this exercise, we will use the Zeek command with the -Cr option to analyze the packet capture file using a specific script (detection-log4j.zeek).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>zeek -Cr log4shell.pcapng detection-log4j.zeek</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After retrieving the log file generated by the Zeek command, we will use zeek-cut to extract the information needed for the following questions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat file.log | zeek-cut </kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log4shell.pcapng file with detection-log4j.zeek script. Investigate the signature.log file. What is the number of signature hits?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By looking up uid, we can learn how many hits by the script.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1023,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-328.png?w=1024" alt="" class="wp-image-1023" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the http.log file. Which tool is used for scanning?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Among the list of user agents, Nmap is the tool used for network scanning.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1024,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-329.png?w=886" alt="" class="wp-image-1024" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Investigate the http.log file. What is the extension of the exploit file?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining the URI field and response MIME types, we can identify the names and types of downloaded files.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">“application/x-java-applet” is a MIME type that indicates a file is a Java applet, a small program designed to run within a web browser.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1027,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-330.png?w=886" alt="" class="wp-image-1027" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Investigate the log4j.log file. Decode the base64 commands. What is the name of the created file?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">By reviewing log4j.log, we can find the malicious commands in the URI field.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1029,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-331.png?w=886" alt="" class="wp-image-1029" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":1030,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-332.png?w=886" alt="" class="wp-image-1030" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the previous screenshot, we know the command is encoded in base64. By using the echo command and piping it with <code>base64 --decode</code>, we can extract the original text.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">"echo 'base64 data' | base64 <kbd>--</kbd>decode"</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The decoded output indicates that the attacker used the touch utility to create a file in the /tmp directory. Next, it finds the location of the nc (netcat) executable and writes the path to the file /tmp/pwned. Finally, the attacker establishes a reverse shell connection to the IP address 192.168.56.102 on port 80, executing the /bin/sh shell and providing verbose output.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1036,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-333.png?w=886" alt="" class="wp-image-1036" /></figure>
<!-- /wp:image -->


