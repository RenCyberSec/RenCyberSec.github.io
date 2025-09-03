---
layout: post
title: Phishing - Snapped Phish-ing Line
subtitle: Phishing Email, malicious attachment inspection
date: 2024-10-05 00:02
author: Ren Sie
comments: true
category: threat-hunting
---

{: .box-success}
 Refer to [Snapped Phish-ing Line](https://tryhackme.com/r/room/snappedphishingline) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">As an IT department personnel at SwiftSpend Financial, one of your responsibilities is to assist employees with their technical concerns. While everything seemed routine, the situation changed when several employees from various departments began reporting an unusual email they had received. Unfortunately, some employees had already submitted their credentials and were unable to log in as a result.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"style":{"color":{"text":"#74103e"},"elements":{"link":{"color":{"text":"#74103e"}}}},"fontSize":"large"} -->
<h1 class="wp-block-heading has-text-color has-link-color has-large-font-size" style="color:#74103e">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">We have now begun investigating the situation by:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Analyzing the email samples provided by the colleagues.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Examining the phishing URL(s) by browsing them using Firefox.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Retrieving the phishing kit used by the adversary.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Utilizing CTI-related tools to gather more information about the adversary.</li>
<!-- /wp:list-item -->

<!-- wp:list-item {"fontSize":"small"} -->
<li class="has-small-font-size">Analyzing the phishing kit to collect additional details about the adversary.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Who is the individual who received an email attachment containing a PDF?</strong><br>By reviewing all available emails, we found four .html attachments and one .pdf attachment.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2812,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-80.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-80.png?w=975" alt="" class="wp-image-2812" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What email address was used by the adversary to send the phishing emails?</strong><br>After examining all the emails, we found that all the phishing messages originated from the same email address.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2813,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-81.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-81.png?w=975" alt="" class="wp-image-2813" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the redirection URL to the phishing page for the individual Zoe Duncan?</strong><br>After opening the email to Zoe, I noticed that there was no URL link in the content. It's likely that the link is contained in the attachment. After saving the file to the desktop, we will open it with a text editor instead of directly opening it, as that would not be safe.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>~/Desktop$ gedit &lt;attachment file&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once we view the source code with the text editor, we know the redirect URL.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** When a user visits the .html, they will be instantly redirected to the specified URL with the provided email and error parameters

<!-- wp:image {"id":2815,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-82.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-82.png?w=975" alt="" class="wp-image-2815" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the URL to the .zip archive of the phishing kit?</strong><br>After using the strings and grep commands to search for any 'http' links in the source code, I found an address from the .pdf attachment that matches the one we observed in the .html attachment.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>~/Desktop$ strings &lt;attachment&gt; | grep -e 'http'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2816,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-83.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-83.png?w=975" alt="" class="wp-image-2816" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Based on the previous screenshot, it points to a file named "office365." I removed a couple of directories from the URL, so we will now navigate to the "data" directory instead of downloading the file. </p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** Ensure to use http instead https in the URL.

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>hxxp[://]kennaroads[.]buzz/data/</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After navigating to the address, I found a .zip archive. To obtain the full URL of the archive, right-click on the file and select "Copy link."</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2818,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-84.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-84.png?w=975" alt="" class="wp-image-2818" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the SHA256 hash of the phishing kit archive?</strong><br>To know its hash value, first we need to download it. Then use sha256sum command.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>~/Download$ sha256sum &lt;file.zip&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2820,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-85.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-85.png?w=975" alt="" class="wp-image-2820" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>When was the phishing kit archive first submitted?</strong><br>By "first submitted date," it refers to the date when the file was uploaded to the community for analysis. We can know the date from <a href="https://www.virustotal.com/gui/file/ba3c15267393419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686/details">VirusTotal</a>, simply upload the hash value that we retrieved from the previous question.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2821,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-86.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-86.png?w=975" alt="" class="wp-image-2821" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>When was the SSL certificate the phishing domain used to host the phishing kit archive first logged?</strong><br>To learn about the SSL certificate for the domain, we can search for it on <a href="https://crt.sh/?q=kennaroads.buzz">crt.sh</a>. After performing the search, we will receive a list of the SSL certificate history for the domain, including the first certificate.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** crt.sh allows users to search and view details of SSL/TLS certificates from Certificate Transparency logs.

<!-- wp:image {"id":2823,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-87.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-87.png?w=975" alt="" class="wp-image-2823" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What was the email address of the user who submitted their password twice?</strong><br>While searching for the .zip archive, I came across with the log.txt file.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2825,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-88.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-88.png?w=975" alt="" class="wp-image-2825" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once I opened the file, I identified the user who submitted their credentials twice.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2826,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-89.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-89.png?w=975" alt="" class="wp-image-2826" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What was the email address used by the adversary to collect compromised credentials?</strong><br>To find the email address used to collect the credentials, we need to review the toolkit. Once we unzip the file, we'll navigate to its folder.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2827,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-90.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-90.png?w=975" alt="" class="wp-image-2827" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will use the grep command to search for any keywords related to the email address.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>~/Downloads/Update365/office365$ grep -rni ".com" *</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2828,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-91.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-91.png?w=975" alt="" class="wp-image-2828" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The adversary used other email addresses in the obtained phishing kit. What is the email address that ends in "@gmail.com"?</strong><br>We will use the grep command to search for the Gmail address contained in the toolkit.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>~/Downloads/Update365/office365$ grep -rni "gmail.com" *</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2830,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-92.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-92.png?w=975" alt="" class="wp-image-2830" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the hidden flag?</strong><br>Based on the provided hint, I will add "flag.txt" to the URL in each directory.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2831,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-93.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-93.png?w=740" alt="" class="wp-image-2831" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Finally, I found another hint in one of the directories. It mentioned that this needs to be decoded in base64 after being input into ChatGPT.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2832,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-94.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-94.png?w=840" alt="" class="wp-image-2832" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">My first attempt at decoding revealed that the order of the characters was incorrect, so I used the rev command to reverse it and successfully retrieved the flag.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2833,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-95.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-95.png?w=975" alt="" class="wp-image-2833" /></a></figure>
<!-- /wp:image -->

