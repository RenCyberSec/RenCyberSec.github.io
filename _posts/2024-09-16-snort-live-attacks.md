---
layout: post
title: Snort - Live Attacks
subtitle: Analyze network traffic to identify attacker patterns and write Snort rules for detecting known attack vectors (Brute-Force and Reverse Shell).
date: 2024-09-16 17:00
author: Ren Sie
comments: true
category: network-traffic-inspection
---

{: .box-success}
 Refer to [Live Attacks](https://tryhackme.com/room/snortchallenges2) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario #1, Brute-force</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The company is under the brute-force attack, observe the traffic with Snort and identify the anomaly first. Then creating a rule to stop the attack.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Start Snort in sniffer mode and figure out the attack source, service and port.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">A few points to remember:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Create the rule and test it with "-A console" mode.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Use "-A full" mode and the default log path to stop the attack.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Write the correct rule and run the Snort in IPS "-A full" mode.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Block the traffic for a minute and then the flag file will appear on the desktop.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Capture the Traffic</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By using option -A console, traffic will be displayed at console. Stop the capture (CRTL+C) in 30 seconds, examine the traffic from the console for the challenge questions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c /etc/snort/snort.conf -v <strong>-A console</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Analyze the Traffic</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After executing the recommended command, I observed continuous traffic originating from and directed towards two distinct end devices, which suggests a possible brute force attack. The figure below illustrates that the attack is targeting our web server.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":746,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-222.png?w=1024" alt="" class="wp-image-746" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">I then wrote summary note with Mousepad.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":748,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-223.png?w=945" alt="" class="wp-image-748" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">The figure below shows that the attack is targeting the SSH service.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":750,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-224.png?w=1024" alt="" class="wp-image-750" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">I then wrote summary note with Mousepad.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":752,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-225.png?w=945" alt="" class="wp-image-752" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Create the Rule to Block Malicious Traffic</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the findings from the previous section, I created two rules to address the source address and service port on the victim. Add the customized rule to the local rule file located at <code>/etc/snort/rules/local.rules</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule #1: alert tcp 10.10.245.36 any -&gt; 10.10.140.29 <strong>22 </strong>(msg:"Malicious SSH Connection!"; sid:100001; rev:1;)</kbd><br><kbd>Rule #2: alert tcp 10.100.2.28 any -&gt; 10.10.74.169 <strong>80 </strong>(msg:"Malicious HTTP Connection!"; sid:100002; rev:1;)</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Test the New Rule</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Before capturing and blocking the target traffic, we should ensure that the customized rule is configured correctly by running a test.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c /etc/snort/snort.conf -T <strong>-A console</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":758,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-227.png?w=1024" alt="" class="wp-image-758" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":759,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-228.png?w=1024" alt="" class="wp-image-759" /></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Implement the Rule in IPS Mode</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To run IPS mode, we need to add some options to the command;</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-q” makes Snort less verbose, showing fewer informational messages.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-Q” enables Inline mode, so Snort will actively block or drop suspicious traffic.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“--daq afpacket” afpacket module, which supports Inline mode.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-i eth0:eth1” specifies the network interfaces eth0 and eth1 to monitor.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-A full” means Snort will provide complete details about any detected issues.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-l /var/log/snort/” specifies Snort should store its logs to default log path.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 <strong>-A</strong> full -l /var/log/snort/</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Capture the Flag</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After allowing the IPS to run for a minute, a flag.txt file will appear on the desktop.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":765,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-229.png?w=1024" alt="" class="wp-image-765" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario #2, Reverse-Shell</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We have stopped some inbound access attempts. However, there’s attacker who’s already inside of our network, As well as the insider risks. The dwell time is still around 1-3 months, so it is worth checking the outgoing traffic. We’ve notice there’s persistent outbound traffic is detected. Possibly a reverse shell.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Capture the Traffic</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By using option -A console, traffic will be displayed at console. Stop the capture (CRTL+C) in 30 seconds, examine the traffic from the console for the challenge questions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c /etc/snort/snort.conf -v <strong>-A console</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Analyze the Traffic</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">As soon as I see the traffic come in and go out from port 4444, I automatically associated it with the reverse shell generated on Metasploit. The screenshot below shows the Reverse Shell connection to malicious node.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":770,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-230.png?w=1024" alt="" class="wp-image-770" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I then wrote a summary note with Mousepad.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":772,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-231.png?w=1024" alt="" class="wp-image-772" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Create the Rule to Block Malicious Traffic</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the findings from the previous section, I created a rule to address the source address and service port on the victim. Add the customized rule to the local rule file located at /etc/snort/rules/local.rules.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>Rule #1: alert tcp 10.10.144.156 4444 -&gt; 10.10.196.55 any (msg:"Reverse Shell Connection!"; sid:100001; rev:1;)</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Test the New Rule</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Before capturing and blocking the target traffic, we should ensure that the customized rule is configured correctly by running a test.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c /etc/snort/snort.conf -T <strong>-A console</strong></kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":776,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-232.png?w=1024" alt="" class="wp-image-776" /></figure>
<!-- /wp:image -->

<!-- wp:image {"id":777,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-233.png?w=1024" alt="" class="wp-image-777" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Implement the Rule in IPS Mode</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To run IPS mode, we need to add some options to the command;</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-q” makes Snort less verbose, showing fewer informational messages.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-Q” enables Inline mode, so Snort will actively block or drop suspicious traffic.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“--daq afpacket” afpacket module, which supports Inline mode.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-i eth0:eth1” specifies the network interfaces eth0 and eth1 to monitor.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-A full” means Snort will provide complete details about any detected issues.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">“-l /var/log/snort/” specifies Snort should store its logs to default log path.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>sudo snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 <strong>-A</strong> full -l /var/log/snort/</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Capture the Flag</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After allowing the IPS to run for a minute, a flag.txt file will appear on the desktop.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":779,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-234.png?w=1024" alt="" class="wp-image-779" /></figure>
<!-- /wp:image -->

