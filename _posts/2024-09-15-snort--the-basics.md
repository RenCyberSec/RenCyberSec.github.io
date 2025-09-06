---
layout: post
title: Snort - The Basics
subtitle: Understand Snort alerts and write Snort rules to detect known cyber threats.
date: 2024-09-15 18:52
author: Ren Sie
comments: true
category: network-traffic-inspection
---

{: .box-success}
 Refer to [The Basics](https://tryhackme.com/room/snortchallenges1) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Writing IDS Rules (HTTP)</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Write a single rule to detect "all TCP port 80 traffic" packets in the given pcap file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Number of detected packets?</strong><br>Since the question pertains to HTTP traffic (TCP port 80), the rule should specify tcp port 80.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any &lt;&gt; any <strong>80</strong> (msg: "HTTP Packet Found!"; sid:100008; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":624,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-200.png?w=295" alt="" class="wp-image-624" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log file, what is the destination address of packet 63?</strong><br>Using the -n 63 option will limit the output to 63 alerts from specific logs. To identify the destination address, examine the last packet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r &lt;Snort Log Path&gt; <strong>-n 63</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":515,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-124.png?w=1024" alt="" class="wp-image-515" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the ACK number of packet 64?</strong><br>Using the -n 64 option will limit the output to 64 alerts from specific logs. To determine the ACK number, review the 64th packet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r snort.log.1722048011 <strong>-n 64</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":517,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-125.png?w=1024" alt="" class="wp-image-517" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the SEQ number of packets 62?</strong><br>Reuse the output from the previous step and scroll up to the 62nd packet to find the relevant information.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":519,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-127.png?w=1024" alt="" class="wp-image-519" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the TTL of packet 65? Source IP of packet 65? Source port of packet 65?</strong><br>You can address all three questions with a single command.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r snort.log.1722048011 -n 65</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":520,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-128.png?w=1024" alt="" class="wp-image-520" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Writing IDS Rules (FTP)</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">A list of FTP message code can be found in <a href="https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes">List of FTP server return codes</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Detect “all TCP port 21”, Number of detected packets?</strong><br>Since the question requires FTP packets, we should include tcp port 21 in the rules.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any &lt;&gt; any 21 (msg: "FTP Packet Found!"; sid:100008; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt;.pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":623,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-199.png?w=295" alt="" class="wp-image-623" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the FTP service name?</strong><br>Using the <code>-X</code> option allows us to review the full packet details in HEX, including the FTP service name. After exporting the results to a test file, use the search feature (CTRL + F) in your text editor to locate specific information.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r snort.log.1722048928 -X &gt;&gt; test.txt</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":524,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-130.png?w=1024" alt="" class="wp-image-524" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Detect failed FTP login attempts, Number of detected packets?</strong><br>According to the<a href="https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes"> list of FTP server return codes</a>, a failed FTP login returns a 530 service code. Therefore, we should include this code in the rule.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any &lt;&gt; any 21 (msg: "FTP login failed"; <strong>content:"530"</strong>; nocase; sid:100008; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":622,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-198.png?w=295" alt="" class="wp-image-622" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Verify the FTP error message by examining the log to confirm that the failed login packet contains the service code 530.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":527,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-132.png?w=1024" alt="" class="wp-image-527" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Detect successful FTP logins, Number of detected packets?</strong><br>According to the <a href="https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes">list of FTP server return codes</a>, a successful FTP login returns a 230 service code. Therefore, we should include this code in the rule.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any &lt;&gt; any 21 (msg: "FTP login failed"; <strong>content:"230"</strong>; nocase; sid:100008; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":621,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-197.png?w=295" alt="" class="wp-image-621" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Verify the FTP message by examining the log to confirm that the success login packet contains the service code 530.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":530,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-134.png?w=1024" alt="" class="wp-image-530" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Detect FTP login attempts with a valid username but no password entered yet, Number of detected packets?</strong><br>According to the <a href="https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes">list of FTP server return codes</a>, an FTP session with a valid username but no password entered yet returns a 331 service code. Therefore, this code should be included in the rule.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any &lt;&gt; any 21 (msg: "FTP login failed"; <strong>content:"331"</strong>; nocase; sid:100008; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":620,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-196.png?w=295" alt="" class="wp-image-620" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Verify the FTP message by examining the log to confirm that an FTP session with a valid username but no password entered yet returns a 331 service code</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":534,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-136.png?w=1024" alt="" class="wp-image-534" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Detect FTP login attempts with the "Administrator" username but no password entered yet, Number of detected packets?</strong><br>This question is similar to the previous one, but it requires an additional rule to capture login attempts using the "Administrator" username. Therefore, we will use fast_pattern to combine both conditions in the rule.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any &lt;&gt; any 21 (msg: "FTP login failed"; content:"331"; <strong>fast_pattern</strong>; <strong>content:" Administrator"</strong>; nocase; sid:100008; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":619,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-195.png?w=295" alt="" class="wp-image-619" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Verify the FTP message by examining the log to confirm that an FTP session with a username "<kbd><strong>Administrator</strong></kbd>" but no password entered yet returns a 331 service code.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":538,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-138.png?w=1024" alt="" class="wp-image-538" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Writing IDS Rules (PNG)</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Write a rule to detect the PNG file in the given pcap, Investigate the logs and identify the software name embedded in the packet.</strong><br>We will use the <a href="https://www.garykessler.net/library/file_sigs.html">file signature</a> to detect .PNG files.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert ip any any &lt;&gt; any any (msg: ".PNG is Found!"; <strong>content:"|89 50 4E 47 0D 0A 1A 0A|"</strong>; sid:100010; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining the output log, we can determine that the embedded software is Adobe ImageReady, as indicated by the ASCII data.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r &lt;Snort Log Path&gt; -X</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":542,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-140.png?w=1024" alt="" class="wp-image-542" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Write a rule to detect the GIF file in the given pcap, Investigate the logs and identify the image format embedded in the packet.</strong><br>We will use the <a href="https://www.garykessler.net/library/file_sigs.html">file signature</a> to detect .GIF files.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert ip any any &lt;&gt; any any (msg: ".GIF is Found!"; <strong>content:"|47 49 46 38|"</strong>; sid:100011; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":618,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-194.png?w=295" alt="" class="wp-image-618" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r &lt;Snort Log Path&gt; -X</kbd></p>
<!-- /wp:paragraph -->

{: .box-}
**Note:** Setting the signature rule to “47 49 46 38 <strong>37</strong> <strong>61</strong>,” which corresponds to <strong>GIF87a</strong>, will not detect the files, as they are in GIF89a format.

<!-- wp:image {"id":545,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-142.png?w=886" alt="" class="wp-image-545" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Writing IDS Rules (Torrent Metafile)</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Write a rule to detect the torrent metafile in the given pcap.</strong><br>Torrent metafile is a .torrent file containing metadata that describes the files to be shared and their structure on a BitTorrent network. We will use “2E 74 6F 72 72 65 6E 74” to detect the .torrent file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert ip any any &lt;&gt; any any (msg: ".Torrent is Found!"; content:"| 2E 74 6F 72 72 65 6E 74|"; sid:100012; rev:1;)</kbd><br><kbd>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; - A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":617,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-193.png?w=295" alt="" class="wp-image-617" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the name of the torrent application?</strong><br>Using the <code>-X</code> option enables us to review the full packet details in both HEX and ASCII. By examining the ASCII data, we can identify that the application in use is BitTorrent.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -r &lt;Snort Log Path&gt; -X</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:group {"layout":{"type":"flex","orientation":"vertical"}} -->
<div class="wp-block-group"><!-- wp:image {"id":549,"sizeSlug":"large","linkDestination":"none","className":"is-style-default"} -->
<figure class="wp-block-image size-large is-style-default"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-144.png?w=886" alt="" class="wp-image-549" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":550,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-145.png?w=886" alt="" class="wp-image-550" /></figure>
<!-- /wp:image --></div>
<!-- /wp:group -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the MIME (Multipurpose Internet Mail Extensions) type of the torrent metafile?</strong><br>Refer to the previous Figure, the MIME type is application/x-bittorrent.<br>The <strong>MIME type application/x-bittorrent</strong> indicates that the file is a torrent metafile, which BitTorrent programs use to handle and share files. This type of file contains details on how to download and piece together the larger files shared over the BitTorrent network.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the hostname of the torrent metafile?</strong><br>Under the same log, we can see the host information (tracker2.torrentbox.com) within the packet.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":553,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-146.png?w=886" alt="" class="wp-image-553" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Troubleshooting Rule Syntax Errors</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Test each ruleset with following command;</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c local-X.rules -r mx-1.pcap -A console</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Fix the syntax error in local-1.rules file . Number of the detected packets?</strong><br>After running the test, we know there’s error in the syntax “<strong>any(msg:</strong>”</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":555,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-148.png?w=1024" alt="" class="wp-image-555" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After adding a space in between “any (msg<strong>:</strong>”, we have successfully detected the packets.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":616,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-192.png?w=325" alt="" class="wp-image-616" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Fix the syntax error in local-2.rules file . Number of the detected packets?</strong><br>After running the test, we know the error caused by the missing port value in rule (<strong>icmp any -&gt; any any</strong>).</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":558,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-151.png?w=1024" alt="" class="wp-image-558" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After adding a port value after headers “icmp any <strong>any</strong> -&gt; any any”,  we have successfully detected the packets.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":615,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-191.png?w=295" alt="" class="wp-image-615" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Fix the syntax error in local-3.rules file . Number of the detected packets?</strong><br>After running the test, we know the error caused by the duplicated rule SID (<strong>sid: 1000001</strong>)</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":560,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-153.png?w=1024" alt="" class="wp-image-560" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After adjusting one SID of two rules (sid: 1000002), we have successfully detected the packets.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":563,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-155.png?w=1024" alt="" class="wp-image-563" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":614,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-190.png?w=295" alt="" class="wp-image-614" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Fix the syntax error in local-4.rules file . Number of the detected packets?</strong><br>After running the test, we know the error caused by rule at <strong>line 9</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":565,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-157.png?w=886" alt="" class="wp-image-565" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining the rules, we know that there’s syntax error and SID duplication.<br>After fixing the syntax (: &gt; ;) and adjusting one SID of two rules (sid: 1000002), we have successfully detected the packets.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":568,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-158.png?w=886" alt="" class="wp-image-568" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":613,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-189.png?w=295" alt="" class="wp-image-613" /></figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Fix the syntax error in local-5.rules file . Number of the detected packets?</strong><br>After running the test, we know the error caused by direction specifier (<strong>&lt;-</strong>).</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** There is no "&lt;-" operator in Snort, only "-&gt;" Source to destination flow and "&lt;&gt;" Bidirectional flow.

<!-- wp:image {"id":572,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-161.png?w=886" alt="" class="wp-image-572" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After fixing the syntax error, I receive another error alert.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":574,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-163.png?w=1024" alt="" class="wp-image-574" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":575,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-164.png?w=886" alt="" class="wp-image-575" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After fixing the syntax error, I receive another error alert from line 10. Which is the same error as local-4.rules file.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":577,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-165.png?w=886" alt="" class="wp-image-577" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":578,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-166.png?w=886" alt="" class="wp-image-578" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After fixing the syntax (<strong>: &gt; ;</strong>), we have successfully detected the packets.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":612,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-188.png?w=295" alt="" class="wp-image-612" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Fix the logical error in local-6.rules file &nbsp;to create alerts. Number of the detected packets?</strong><br>After running the test, we notice there’s <strong>no captured packet</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":611,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-187.png?w=295" alt="" class="wp-image-611" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By studying the rule message, we know that this is meant for detecting <strong>HTTP GET Request</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":583,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-169.png?w=886" alt="" class="wp-image-583" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After modifying the options (<strong>content:”GET”;nocase</strong>), we have successfully detected the packets.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":585,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-170.png?w=886" alt="" class="wp-image-585" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":610,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-186.png?w=295" alt="" class="wp-image-610" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Additionally, I run <kbd><strong>sudo snort -c local-6.rules -r mx-1.pcap -A full -l .</strong></kbd> for verifying the GET Requet rule. Read the Snort Log with -X option, we verify that the new rule successfully capture the HTTP GET Request.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":588,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-172.png?w=886" alt="" class="wp-image-588" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Fix the logical error in local-7.rules file to create alerts. What is the name of the required option:</strong><br>By studying the rule, we notice the message is missing. Judging by the file signature 2E 68 74 6D 6C, we know it’s looking for .html</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":589,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-173.png?w=886" alt="" class="wp-image-589" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Adding the msg option.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":590,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-174.png?w=886" alt="" class="wp-image-590" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Using External Rules (MS17-010)</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">MS17-010 is a critical security update released by Microsoft to address vulnerabilities in the Server Message Block (SMB) protocol. It’s associated with the "EternalBlue" exploit used in the WannaCry ransomware attack.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Use the given rule file (local.rules) to investigate the ms1710 exploitation, what is the number of detected packets?</strong><br>Since the question only addresses alterations, there is no need to export a log. Use the -A console option to output the results to the terminal instead of a log file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c &lt;Local Rule Path&gt; -r &lt;.pcap Path&gt; <strong>-A console</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":609,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-185.png?w=295" alt="" class="wp-image-609" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Write a rule to detect payloads containing the "\IPC$" keyword. What is the number of detected packets?</strong><br>To discover the keyword \IPC$, we will include content:"keyword" in our rule. Since the backslash "\" is an escape character, it must be escaped with another backslash to be correctly interpreted in the content string.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** MS17-010 addressed the flaws that allowed remote code execution through the \IPC$ share and other SMB-related functionalities.

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert ip any any &lt;&gt; any any (msg: "\\IPC$ Found!"; <strong>content:"\\IPC$"</strong>; nocase; sid:1000001; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; -A console</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":608,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-184.png?w=295" alt="" class="wp-image-608" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Examining the log to ensure the packet contains \IPC$.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":595,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-177.png?w=1024" alt="" class="wp-image-595" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log/alarm files, what is the requested path?</strong><br>Refer to the previous log screenshot; the requested path is \\&lt;IP Address&gt;\IPC$.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log/alarm files, what is the CVSS v2 score of the MS17-010 vulnerability?</strong><br>To find the answer, search for the "CVSS v2 score of the MS17-010 vulnerability" from the browser.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":597,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-178.png?w=591" alt="" class="wp-image-597" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":598,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-179.png?w=336" alt="" class="wp-image-598" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"large"} -->
<p class="has-large-font-size">Using External Rules (Log4j)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The flaw in Log4j (ver. 2.0 - 2.14.1) was a remote code execution vulnerability (CVE-2021-44228) that allowed attackers to execute arbitrary code on servers by sending specially crafted log messages.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Use the given rule file (local.rules) to investigate the log4j exploitation, what is the number of detected packets?</strong><br>By utilizing the provided rules, we can identify the detected packets.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; -A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":607,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-183.png?w=295" alt="" class="wp-image-607" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log files, how many rules were triggered?</strong><br>By examining the log from the previous question, we should find the triggered events under the Action Stats (Alerts) section.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; -A console</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":606,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-182.png?w=295" alt="" class="wp-image-606" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Investigate the log files, what are the first six digits of the triggered rule SIDs?</strong><br>We should be able to find the triggered SIDs before the "snort exiting" line.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c &lt;Local Rule Path&gt; -r &lt; .pcap Path&gt; -A console</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":627,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-201.png?w=1024" alt="" class="wp-image-627" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Write a rule to detect packet payloads between 770 and 855 bytes, what is the number of detected packets?</strong><br>To detect packet payloads between 770 and 855 bytes, add dsize: 770 &lt;&gt; 855 to the rule.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule: alert tcp any any -&gt; any any (msg:"File Size Between 770 and 855 Bytes Found!"; <strong>dsize:771&lt;&gt;856</strong>; sid:2000002; rev:1;)<br>sudo snort -c &lt;Local Rule Path&gt; -r &lt;.pcap Path&gt; -A full -l .</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":630,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-202.png?w=295" alt="" class="wp-image-630" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log files, what is the name of the used encoding algorithm?</strong><br>After skimming through all the captured packets, I found the encoding algorithm in the packet with ID: 62808.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">sudo snort &nbsp;-r &lt; Snort-Log-Path &gt; -X</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":632,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-203.png?w=1024" alt="" class="wp-image-632" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":633,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-204.png?w=1024" alt="" class="wp-image-633" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Investigate the log files, what is the IP ID of the corresponding packet?</strong><br>Refer to the previous question to identify its IP address.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>Decode the encoded command, what is the attacker's command?</strong><br>Use the strings tool and pipe the output to grep to capture the desired content.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":636,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-205.png?w=1024" alt="" class="wp-image-636" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Then Use <a href="https://cyberchef.org/">Cyberchef </a>to decode the command. By studying the decoded command, we know that the attacker tried to fetch and execute a script from the URL 45.155.205.233:5874 / 162.0.228.253:80 using curl or wget, and then pipes the retrieved content directly to bash to be executed as a shell script.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":638,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-206.png?w=1024" alt="" class="wp-image-638" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the CVSS v2 score of the Log4j vulnerability?</strong><br>Search for "CVSS 2.0 Log4j vulnerability" in your browser to find the CVSS score.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":639,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-207.png?w=1024" alt="" class="wp-image-639" /></figure>
<!-- /wp:image -->

