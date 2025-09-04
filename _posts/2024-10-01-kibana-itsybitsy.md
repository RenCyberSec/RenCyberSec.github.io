---
layout: post
title: Kibana - ItsyBitsy
subtitle: Tracing logs for suspicious activities on Kibana
date: 2024-10-01 18:38
author: Ren Sie
comments: true
category: threat-hunting
---

{: .box-success}
 Refer to [ItsyBitsy](https://tryhackme.com/r/room/itsybitsy) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">While monitoring security, Analyst John saw an alert from the IDS system about possible command-and-control (C2) communication involving a user named <strong>Browne</strong> from HR. A suspicious file with a known malicious pattern was accessed. We’ve pulled a week’s worth of HTTP connection logs to investigate, but due to limited resources, we only have these logs in Kibana (Index: <strong>connection_logs</strong>). Our job is to review the network connection logs for this user, identify the link and content of the suspicious file.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After logging into the Elastic home, click on the hamburger icon and navigate to the  Discover under the Analytics section. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2568,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-27.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-27.png?w=943" alt="" class="wp-image-2568" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>How many events were returned for the month of March 2022?</strong><br>By setting the date range to March 1, 2022, to April 1, 2022, as specified in the question, we can determine the number of events that occurred during that time period.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2557,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-22.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-22.png?w=945" alt="" class="wp-image-2557" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the IP associated with the suspected user in the logs?</strong><br>I began the investigation by examining the source IP. Since one address accounts for only 0.4% of all the logs, I decided to start there to check for any suspicious activity.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2569,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-28.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-28.png?w=472" alt="" class="wp-image-2569" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">When seeing a user from the internal network retrieving data from Pastebin, it indicates the suspicious activity.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2574,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-30.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-30.png?w=1024" alt="" class="wp-image-2574" /></a></figure>
<!-- /wp:image -->

{: .box-note}
**Note:** Pastebin is used for sharing snippets of code, configuration files, or other text.

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?</strong><br>Refer to the previous screenshot for the user agent used to retrieve the file from the suspicious IP address identified earlier.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?</strong><br>As mentioned in the second question, Pastebin is frequently used to share snippets of code, configuration files, or other text-based content.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the full URL of the C2 to which the infected host is connected?</strong><br>Refer to the previous screenshot. The full URL is the combination of the host (pastebin.com) and the URI (/yTg0Ah6a) because the host identifies the server, while the URI specifies the exact location of the resource on that server.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>A file was accessed on the filesharing site. What is the name of the file accessed?</strong><br>To discover the file shared on Pastebin, enter the full URL that we discovered from the previous question into a web browser. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2562,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-25.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-25.png?w=945" alt="" class="wp-image-2562" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The file contains a secret code with the format THM{_____}.</strong><br>The flag was mentioned in the previous question as well.</p>
<!-- /wp:paragraph -->

