---
layout: post
title: Windows forensics - Unattended
subtitle: Inspect a compromised Windows machine's activities with Registry Explorer and Autopsy
date: 2024-09-25 21:59
author: Ren Sie
comments: true
category: digital-forensics
---

{: .box-success}
 Refer to [Unattended](https://tryhackme.com/r/room/unattended) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Our client has a new employee who observed a suspicious janitor leaving his office as he returned from lunch. &nbsp;Investigate user activity between <strong>12:05 PM and 12:45 PM on November 19, 2022</strong>. Identify any accessed and potentially exfiltrated files.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Use the disk image located at <strong>C:\Users\THM-RFedora\Desktop\kape-results\C</strong> for the investigation. The tools can be found at <strong>C:\Users\THM-RFedora\Desktop\tools</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Snooping around</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Initial investigations show that someone accessed the user's computer during the specified timeframe. It seems this individual knew exactly what to look for, which raises some questions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will use Registry Explorer to See a user's recent activity by checking the paths they’ve typed into the Windows Explorer address bar:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\<strong>WordWheelQuery</strong></kbd></p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** WordWheelQuery allows users to filter and search results by selecting keywords from file names or attributes.

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What file type was searched for using the search bar in Windows Explorer?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">After navigating to the <kbd>\<strong>WordWheelQuery</strong></kbd>, we will see the file type searched in the search bar.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1981,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-694.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-694.png?w=1024" alt="" class="wp-image-1981" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What top-secret keyword was searched for using the search bar in Windows</strong> <strong>Explorer?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">The searched keyword is also included in the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Can't simply open it</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Unsurprisingly, they found what they were looking for within minutes. However, they encountered an obstacle and needed additional information to proceed.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">TIP: To improve load times in the Autopsy Tool, select "Recent Activity" in the Ingest settings.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">In this section, we will use Autopsy to address the question. First, create a case using the data source from the logical file (C:\Users\THM-RFedora\Desktop\kape-results\C\). As suggested, select only "Recent Activity" in the ingest settings.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>What is the name of the downloaded file to the Downloads folder?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Navigate to the Web Downloads tab for downloaded files. As noted in the introduction, we need to <strong>investigate user activity between 12:05 PM and 12:45 PM on November 19, 2022</strong>. Search for any files downloaded within that period and located in the user’s download directory.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1983,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-695.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-695.png?w=1024" alt="" class="wp-image-1983" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>When was the file from the previous question downloaded?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Refer to the previous screenshot for the download time.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Thanks to the previously downloaded file, a PNG file was opened. When was this file opened?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">I found two ways to answer this question: using Registry Explorer or Autopsy. Navigate to NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">On Registry Explorer</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1985,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-696.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-696.png?w=1024" alt="" class="wp-image-1985" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">On Autopsy</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1986,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-697.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-697.png?w=1024" alt="" class="wp-image-1986" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Sending it outside</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Uh oh. They've found valuable data and are now preparing to exfiltrate it from the network. Since USB is not an option, what alternative methods might they use?</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>A text file was created in the Desktop folder. How many times was this file opened?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">As the hint suggested, we shall investigate into <strong>Jumplist</strong> with <strong>JLECmd</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-tertiary-background-color has-background has-small-font-size"><kbd>JLECmd.exe -d "C:\Users\THM-RFedora\Desktop\kape-results\C\Users\THM-RFedora\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations" --csv &lt;output-path&gt;</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Once we open the output.csv file with EZ Viewer, look for the "Path" and "InteractionCount" columns. This will provide the information we need.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1988,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-698.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-698.png?w=1024" alt="" class="wp-image-1988" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>When was the text file from the previous question last modified?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Check the "Last Modified" column in the .csv file or refer to the previous screenshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The contents of the file were exfiltrated to pastebin.com. What is the generated URL of the exfiltrated data?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Use the search feature for the keyword "pastebin.com" in Autopsy to find the URL of the exfiltrated data.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1989,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-699.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-699.png?w=738" alt="" class="wp-image-1989" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the string that was copied to the pastebin URL?</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">By pasting the URL found in the previous question into the browser, we can view the content.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1990,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-700.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-700.png?w=1004" alt="" class="wp-image-1990" /></a></figure>
<!-- /wp:image -->


