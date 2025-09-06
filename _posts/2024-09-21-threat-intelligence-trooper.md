---
layout: post
title: Threat Intelligence - Trooper
subtitle: APT Information gathering on OpenCTI, ATT&CK
date: 2024-09-21 16:04
author: Ren Sie
comments: true
category: threat-intelligence
---

{: .box-success}
 Refer to [Trooper](https://tryhackme.com/r/room/trooper) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">A multinational technology company has faced multiple cyber attacks in recent months, resulting in the theft of sensitive intellectual property and disruptions to operations. A threat advisory report on these attacks has been shared.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">As CTI analysts, our objective is to identify the tactics, techniques, and procedures (TTPs) used by the threat group, and to gather information about their identity and motives. We will use the OpenCTI platform and the MITRE ATT&amp;CK navigator for this analysis.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What kind of phishing campaign does APT X use as part of their TTPs?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The threat group has used phishing to gain initial access. According to the report on <a href="https://documents.trendmicro.com/assets/wp/wp-operation-tropic-trooper.pdf">Operation Tropic Trooper</a>, the attackers sent emails with malicious attachments to exploit known vulnerabilities.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1463,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-526.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-526.png?w=1024" alt="" class="wp-image-1463" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the name of the malware used by APT X?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The threat group used a USB worm infection strategy, transferring a malware installer via USB to an air-gapped host machine according to this <a href="https://assets.tryhackme.com/additional/trooper-cti/APT%20X_USBFerry.pdf">report</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1466,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-527.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-527.png?w=945" alt="" class="wp-image-1466" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the malware's STIX ID?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Go to OpenCTI and search for the malware name in the Arsenal tab.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1467,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-528.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-528.png?w=1024" alt="" class="wp-image-1467" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">After accessing the malware details page, we can find its STIX ID.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1469,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-529.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-529.png?w=1024" alt="" class="wp-image-1469" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">With the use of a USB, what technique did APT X use for initial access?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Open the MITRE ATT&amp;CK Navigator to locate the technique in the Initial Access column.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1470,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-530.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-530.png?w=221" alt="" class="wp-image-1470" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the identity of APT X?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The identity of the threat group can be identified in several ways: through the articles provided earlier or by searching for the malware name in the <a href="https://attack.mitre.org/software/S0452/">MITRE ATT&amp;CK</a> database.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1471,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-531.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-531.png?w=1024" alt="" class="wp-image-1471" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">How many Attack Pattern techniques are associated with the APT?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Once we identify the threat group, search for it in OpenCTI under the Threats tab.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1473,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-532.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-532.png?w=1024" alt="" class="wp-image-1473" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Next, go to the Knowledge tab to review its attack pattern.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1474,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-533.png?w=1024" alt="" class="wp-image-1474" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the name of the tool linked to the APT?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Refer to the right panel of the page and select the "Tools" tab.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1475,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-534.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-534.png?w=360" alt="" class="wp-image-1475" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After accessing the list of tools used by the threat group, we will identify the tool associated with the APT.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1476,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-535.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-535.png?w=1024" alt="" class="wp-image-1476" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What is the sub-technique used by the APT under Valid Accounts?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After accessing the ATT&amp;CK Navigator, use the search function to find "Valid Account." Once you locate the technique, click the icon next to it to display the sub-technique.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1478,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-536.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-536.png?w=554" alt="" class="wp-image-1478" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">Under what Tactics does the technique above fall?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To determine the tactics associated with the "Valid Accounts" technique, use the search function. We will see four results; check which tactics correspond to each technique.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem">What technique is the group known for using under the tactic Collection?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By locating the technique under the "Collection" column, we can identify the technique used by the threat group.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1479,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-537.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-537.png?w=219" alt="" class="wp-image-1479" /></a></figure>
<!-- /wp:image -->

