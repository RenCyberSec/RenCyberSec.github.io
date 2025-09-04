---
layout: post
title: Linux forensics - Disgruntled
subtitle: Linux logs and suspicious binary exmination
date: 2024-09-26 22:39
author: Ren Sie
comments: true
category: digital-forensics
---

{: .box-success}
 Refer to [Disgruntled](https://tryhackme.com/r/room/disgruntled) for the challenge room on TryHackMe

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">An IT employee from CyberT has been arrested for running a phishing operation. CyberT has requested our assistance to determine if this individual compromised any of their assets.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Nothing suspicious... So far</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Here’s the machine that our dissatisfied IT user last worked on. Please assess if there are any concerns for our client. My suggestion: Review the privileged commands that were executed. This will help you begin your investigation.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>The user installed a package on the machine using elevated privileges. What is the full COMMAND?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">All user authentication activities on a Linux host are recorded in the auth.log file, found at /var/log/auth.log. We can use the grep tool to search for the keywords "COMMAND" and "install," as the question mentioned installing a package.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat /var/log/auth.log* | grep -i 'COMMAND.*install'</kbd></p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** The '.*' in the regex pattern allows for any characters (including none) to appear between "COMMAND" and "install.

<!-- wp:image {"id":1999,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-701.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-701.png?w=1024" alt="" class="wp-image-1999" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>What was the present working directory when the previous command was run?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Refer to the previous screenshot for the present working directory (PWD) at the time the install command was executed.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Let’s see if you did anything bad</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Please continue your assessment. Our dissatisfied IT user was only supposed to install a service on this computer, so check for any commands that are unrelated to that task.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>Which user was created after the package from the previous task was installed?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">There are two commands to add a user in Linux: adduser and useradd. Both commands can be found in the same log file (auth.log), so we will update the keyword to include either adduser or useradd.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat /var/log/auth.log* | grep -i 'COMMAND.*adduser'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2002,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-702.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-702.png?w=1024" alt="" class="wp-image-2002" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>A user was then later given sudo priveleges. When was the sudoers file updated?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will continue to examine the same log file (auth.log), but this time we will change the grep keyword to 'sudo', since the question relates to the sudoer file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Two users used visudo to edit the sudoer file. Referring to the previous screenshot, we know that the compromised user, cybert, installed the package and added the user. Therefore, it is highly likely that this same user edited the sudoer file to grant it-admin user privileges.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** visudo is a command that edits the sudoers file.

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat /var/log/auth.log* | grep -i 'COMMAND.*sudo'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2003,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-703.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-703.png?w=1024" alt="" class="wp-image-2003" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>A script file was opened using the "vi" text editor. What is the name of this file?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We will continue to examine the same log file (auth.log) for any commands that executed vi, as the question suggests. The output indicates that a script file (.sh) was opened with vi.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat /var/log/auth.log* | grep -i 'vi'</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2005,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-704.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-704.png?w=1024" alt="" class="wp-image-2005" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Bomb has been planted. But when and where?</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">The bomb.sh file is a significant concern. While the existence of this file raises suspicion, we still need to investigate its origin and contents. Unfortunately, the file is no longer present.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>What is the command used that created the file bomb.sh?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By analyzing the previous screenshot, we see that bomb.sh is stored in the local directory of the it-admin user. Therefore, we need to navigate to this user's local directory and examine the command history.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>/home/it-admin# cat .bash_history</kbd></p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** The command downloads the file (bomb.sh) from the IP address 10[.]10[.]158[.]38 on port 8080 and saves it as bomb.sh. Because the command curl does not use sudo or any privileged access, so it typically won't generate entries in /var/log/auth.log

<!-- wp:image {"id":2007,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-705.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-705.png?w=1024" alt="" class="wp-image-2007" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>The file was renamed and moved to a different directory. What is the full path of this file now?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We know that vi can edit and save files to different locations, so we can begin by investigating the .viminfo file, which records the history of any activities with vi. From the output, we can see that there is a command history showing that the suspicious file was saved to a new location with a different name.</p>
<!-- /wp:paragraph -->

{: .box-note}
**Note:** We will look for this file in the same user's directory, as we did in the previous question.

<!-- wp:image {"id":2008,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-706.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-706.png?w=1024" alt="" class="wp-image-2008" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>When was the file from the previous question last modified? </strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To find the last modification timestamp, we first need to navigate to the file's location and use the ls command with the --full-time option.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>ls -l --full-time os-update.sh</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2010,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-707.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-707.png?w=1024" alt="" class="wp-image-2010" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>What is the name of the file that will get created when the file from the first question executes?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">To understand the function of the script file, we can investigate its content using the cat command. Based on the screenshot of the script content, we can see that the script uses the echo command to output data into the file we are looking for.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat os-update.sh</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2011,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-708.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-708.png?w=1024" alt="" class="wp-image-2011" /></a></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Following the fuse</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">We have both a file and a motive. The key question now is: how was this file intended to be executed? It’s likely that he planned for it to run at some point.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<h2 class="wp-block-heading" style="font-size:1.5rem"><strong>At what time will the malicious file trigger?</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">At this point, we know that the suspicious file is a logic bomb and is set to be executed at some point. To determine the execution date, we can examine the cron file, which documents scheduled commands in Linux.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">By examining the output, we know that the script will be executed daily at 8 in the morning as the root user.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>cat /etc/crontab</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2013,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/09/image-709.png?w=1024" alt="" class="wp-image-2013" /></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Conclusion</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">Thanks to your insights, we now have a clearer understanding of the intentions of our dissatisfied IT employee. We’ve identified that he downloaded a pre-made script designed to delete all files related to the installed service if the user hasn’t logged in within the last 30 days. This is a classic example of a "logic bomb." Impressive work on your second day! Please let Sophie know I recommended a raise for you.</p>
<!-- /wp:paragraph -->

