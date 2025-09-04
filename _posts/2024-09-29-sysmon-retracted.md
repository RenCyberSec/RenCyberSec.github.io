---
layout: post
title: Sysmon - Retracted
subtitle: Sysmon logs Investigation
date: 2024-09-29 18:00
author: Ren Sie
comments: true
category: threat-hunting
---

{: .box-success}
 Refer to [Retracted](https://tryhackme.com/r/room/retracted) for the challenge room on TryHackMe
 
<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">A Mother's Plea</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">"Thanks for coming. I know you are busy with your new job, but I did not know who else to turn to."<br>"So I downloaded and ran an installer for an antivirus program I needed. After a while, I noticed I could no longer open any of my files. And then I saw that my wallpaper was different and contained a terrifying message telling me to pay if I wanted to get my files back. I panicked and got out of the room to call you. But when I came back, everything was back to normal."<br>"Except for one message telling me to check my Bitcoin wallet. But I don't even know what a Bitcoin is!"<br>"Can you help me check if my computer is now fine?"</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">The Message</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">"As soon as you log in to the computer, you'll see a file on the desktop that's meant for me."<br>"I don't know why that message is there or what it means. Do you have any idea?"</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the full path of the text file containing the "message"?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Right-click on SOPHIE.txt and select "Properties." By adding the file name (SOPHIE.txt) to its location, we can determine its full path.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2301,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-837.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-837.png?w=709" alt="" class="wp-image-2301" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2302,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-838.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-838.png?w=709" alt="" class="wp-image-2302" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What program was used to create the text file?</strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To identify the program used to create the file, we need to examine the Sysmon log. We will focus on processes created between 2024-01-08 14:24:00 and 2024-01-08 14:26:00, based on the file creation timestamp (2024-01-08 14:25:16) shown in the file’s properties.<br><br>Open Event Viewer and navigate to Application and Services -&gt; Microsoft -&gt; Windows -&gt; Sysmon -&gt; Operational. Apply a filter for Event ID 1 (Process Create). The log shows that SOPHIE.txt was created using Notepad.exe at 14:25:30.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2304,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-839.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-839.png?w=945" alt="" class="wp-image-2304" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2305,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-840.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-840.png?w=945" alt="" class="wp-image-2305" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2306,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-841.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-841.png?w=945" alt="" class="wp-image-2306" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the time of execution of the process that created the text file? Timezone UTC</strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">the time of execution of the process was indicated in the previous screenshot,</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Something Wrong</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">"I think something went wrong with my computer when I ran the installer. Now, I can't open my files, and my wallpaper changed to a message asking for payment."<br>"Are you saying the file I downloaded is a virus? But I got it from Google!"</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the filename of this "installer"? (Including the file extension)</strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After determining that the malicious executable was downloaded from the Internet, I applied a filter for Event ID 1 (Process Creation). Then search for logs related to file downloads between 2024-01-08 14:00 and 15:00.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2309,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-842.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-842.png?w=945" alt="" class="wp-image-2309" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2310,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-843.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-843.png?w=944" alt="" class="wp-image-2310" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2311,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-844.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-844.png?w=945" alt="" class="wp-image-2311" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong>What is the download location of this installer?</strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The download location was also indicated in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong>The installer encrypts files and then adds a file extension to the end of the file name. What is this file extension?</strong></strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Without applying any filters, the following 18 logs (Event ID 11) display actions taken on objects by the malicious attacker, with all target files having a .dmp file extension.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2313,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-845.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-845.png?w=709" alt="" class="wp-image-2313" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong>The installer reached out to an IP. What is this IP?</strong></strong></strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The question notes that the installer contacted an IP address. I observed a network connection (Event ID 3) initiated by antivirus.exe.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2315,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-846.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-846.png?w=945" alt="" class="wp-image-2315" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2316,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-847.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-847.png?w=945" alt="" class="wp-image-2316" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2317,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-848.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-848.png?w=945" alt="" class="wp-image-2317" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Back to Normal</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">"So what happened to the virus? It looks like it's gone because all my files are back."</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong>The threat actor logged in via RDP right after the “installer” was downloaded. What is the source IP?</strong></strong></strong></strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The question indicates that a connection was established via RDP (Port 3389). I observed a network connection (Event ID 3) established after 2024-01-08 14:15:00.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2318,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-849.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-849.png?w=945" alt="" class="wp-image-2318" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2319,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-850.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-850.png?w=945" alt="" class="wp-image-2319" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2320,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-851.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-851.png?w=944" alt="" class="wp-image-2320" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong><strong><strong><strong><strong>This other person downloaded a file and ran it. When was this file run? Timezone UTC</strong></strong></strong></strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After reviewing the logs following the attacker’s access to SOPHIE’s machine, I found a process creation log for another downloaded file, decryptor.exe.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2322,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-852.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-852.png?w=945" alt="" class="wp-image-2322" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Doesn't Make Sense</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">"So you're saying someone accessed my computer, messed with my files, and then reversed the changes?"              <br>"That doesn’t make sense. Why would they infect my computer and then fix it?"<br>"Can you help me understand what’s going on?"</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Arrange the following events in order from 1 to 7, based on the occurrence timeline.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>3.) Sophie ran out and reached out to you for help.</strong><br>According to <a href="#_A_Mother's_Plea">A Mother’s Plea</a>, "I panicked and got out of the room to call you. But when I came back, everything was back to normal."</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>1.) Sophie downloaded the malware and ran it.</strong><br>2024-01-08 14:15:00.688</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2324,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-853.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-853.png?w=709" alt="" class="wp-image-2324" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>6.) A note was created on the desktop telling Sophie to check her Bitcoin.</strong><br>2024-01-08 14:25:30.749</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2325,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-854.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-854.png?w=709" alt="" class="wp-image-2325" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>5.) The intruder downloaded a decryptor and decrypted all the files.</strong><br>2024-01-08 14:24:18.804</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2326,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-855.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-855.png?w=709" alt="" class="wp-image-2326" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2327,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-856.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-856.png?w=709" alt="" class="wp-image-2327" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>1.) The malware encrypted the files on the computer and showed a ransomware note.</strong><br>2024-01-08 14:15:00.885</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2328,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-857.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-857.png?w=748" alt="" class="wp-image-2328" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>4.) Someone else logged into Sophie's machine via RDP and started looking around.</strong><br>2024-01-08 14:19:20.300</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2331,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-860.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-860.png?w=709" alt="" class="wp-image-2331" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2330,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-859.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-859.png?w=709" alt="" class="wp-image-2330" /></a></figure>
<!-- /wp:image -->

<!-- wp:image {"id":2332,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-861.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-861.png?w=709" alt="" class="wp-image-2332" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>7.) We arrive on the scene to investigate.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Conclusion</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">"Adelle from Finance just called me. She says that someone just donated a huge amount of bitcoin to our charity's account!"<br>"Could this be our intruder? His malware accidentally infected our systems, found the mistake, and retracted all the changes?"<br>"Maybe he had a change of heart?"</p>
<!-- /wp:paragraph -->

