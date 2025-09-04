---
layout: post
title: Splunk Challenge Room
date: 2024-10-01 17:14
author: yurenjoeysie
comments: true
categories: [Endpoint Monitoring, Incident Response, logs, Network Security, Registry Key, SIEM, Splunk, Sysmon, TryHackMe Challenge Rooms, Windows Event Logs, Windows System]
---
<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">Click <a href="https://tryhackme.com/r/room/investigatingwithsplunk">here</a> to enter the challenge room on Try Hack Me</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Scenario</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size">SOC Analyst Johny noticed unusual activity in the logs from several Windows machines. It seems that an attacker has gained access to these machines and set up backdoors. Johny's manager has asked him to collect the logs from these suspected machines and add them to Splunk for a quick review.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1,"fontSize":"large"} -->
<h1 class="wp-block-heading has-large-font-size">Task</h1>
<!-- /wp:heading -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><strong>How many events were collected and Ingested in the index main?</strong><br>An index is a repository where data is stored and organized for easy retrieval and analysis. By querying the index name, we can view the events contained within it.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>The adversary was successful in creating a backdoor user. What is the new username?</strong><br>Searching for Event ID 4720 which logs the creation of a new user account in Active Directory.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main EventID="4720"<br>| table Hostname TargetUserName</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2526,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-15.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-15.png?w=1024" alt="" class="wp-image-2526" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?</strong><br>We already had the hostname and username. Additionally, Event ID 12 logs modifications to registry keys. By querying the keyword, we can determine the full path.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main Hostname="Micheal.Beaven" EventID="12" A1berto<br>| table Category Hostname EventID TargetObject</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2528,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-16.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-16.png?w=1024" alt="" class="wp-image-2528" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>Examine the logs and identify the user that the adversary was trying to impersonate.</strong><br>By using "dedup User" to eliminate duplicate user entries, we can identify all unique user entries in this index, as well as the impersonation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main User="*"<br>| dedup User<br>| table User</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2530,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-17.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-17.png?w=452" alt="" class="wp-image-2530" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the command used to add a backdoor user from a remote computer?</strong><br>By searching for the keyword "username" and displaying the matching results in a table, we can easily identify the command used to create the backdoor.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main A1berto<br>| table Commandline</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size">NOTE: The screenshot displays two commands for creating a user. The first is a WMIC command creates "net user /add A1berto paw0rd1", which facilitates the <strong>remote execution</strong> of commands using Windows Management Instrumentation (WMI). In contrast, the second command is a local net user command, net user /add A1berto paw0rd1, which is <strong>executed locally</strong> and does not support remote user management.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2532,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-18.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-18.png?w=1024" alt="" class="wp-image-2532" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>How many times was the login attempt from the backdoor user observed during the investigation?</strong><br>An examination of all user activities logged by the Windows Event Log or Sysmon shows that the newly created user "A1berto" has not generated any activity.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main User="*"<br>| dedup User<br>| table User</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>What is the name of the infected host on which suspicious Powershell commands were executed?</strong><br>By searching for the keyword "PowerShell" and its associated hostname, we determine the host that executed the suspicious PowerShell command.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main PowerShell<br>| dedup Hostname<br>| table Hostname</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2536,"sizeSlug":"large","linkDestination":"media","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><a href="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-19.png"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-19.png?w=451" alt="" class="wp-image-2536" /></a></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>PowerShell logging is enabled on this device. How many events were logged for the malicious PowerShell execution?</strong><br>PowerShell execution is logged in Event IDs 4103 and 4104.<br><strong>4103</strong> captures the initiation of a PowerShell transcription session, which records all commands and outputs to a text file.<br><strong>4104</strong> documents the execution of a PowerShell script block, offering detailed information about the specific script content and the context in which it was executed. Together, these logs provide valuable insights into PowerShell activity.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"align":"justify","backgroundColor":"tertiary","fontSize":"small"} -->
<p class="has-text-align-justify has-tertiary-background-color has-background has-small-font-size"><kbd>index=main EventID="4103"</kbd></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2538,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-20.png?w=585" alt="" class="wp-image-2538" /></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"justify","fontSize":"small"} -->
<p class="has-text-align-justify has-small-font-size"><strong>An encoded Powershell script from the infected host initiated a web request. What is the full URL?</strong><br>Upon investigating the results from the previous query (<kbd>index=main EventID="4103"</kbd>), I noticed a PowerShell command (powershell.exe -noP -sta -w 1 -enc) that was executed in hidden mode without loading the user profile and utilized base64 encoding.<br>We have determined that the command is encoded. By pasting it into <a href="https://cyberchef.org/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)Remove_null_bytes()&amp;input=U1FCR0FDZ0FKQUJRQUZNQVZnQmxBSElBVXdCSkFHOEFiZ0JVQUdFQVlnQk1BR1VBTGdCUUFGTUFWZ0JGQUhJQVV3QkpBRThBVGdBdUFFMEFZUUJLQUU4QVVnQWdBQzBBUndCbEFDQUFNd0FwQUhzQUpBQXhBREVBUWdCRUFEZ0FQUUJiQUhJQVpRQkdBRjBBTGdCQkFGTUFjd0JsQUUwQVlnQnNBSGtBTGdCSEFHVUFkQUJVQUhrQVVBQkZBQ2dBSndCVEFIa0Fjd0IwQUdVQWJRQXVBRTBBWVFCdUFHRUFad0JsQUcwQVpRQnVBSFFBTGdCQkFIVUFkQUJ2QUcwQVlRQjBBR2tBYndCdUFDNEFWUUIwQUdrQWJBQnpBQ2NBS1FBdUFDSUFSd0JGQUZRQVJnQkpBR1VBWUFCc0FHUUFJZ0FvQUNjQVl3QmhBR01BYUFCbEFHUUFSd0J5QUc4QWRRQndBRkFBYndCc0FHa0FZd0I1QUZNQVpRQjBBSFFBYVFCdUFHY0Fjd0FuQUN3QUp3Qk9BQ2NBS3dBbkFHOEFiZ0JRQUhVQVlnQnNBR2tBWXdBc0FGTUFkQUJoQUhRQWFRQmpBQ2NBS1FBN0FFa0FSZ0FvQUNRQU1RQXhBRUlBWkFBNEFDa0Fld0FrQUVFQU1RQTRBRVVBTVFBOUFDUUFNUUF4QUVJQVJBQTRBQzRBUndCbEFIUUFWZ0JoQUV3QVZRQkZBQ2dBSkFCdUFGVUFiQUJNQUNrQU93QkpBR1lBS0FBa0FFRUFNUUE0QUdVQU1RQmJBQ2NBVXdCakFISUFhUUJ3QUhRQVFnQW5BQ3NBSndCc0FHOEFZd0JyQUV3QWJ3Qm5BR2NBYVFCdUFHY0FKd0JkQUNrQWV3QWtBRUVBTVFBNEFHVUFNUUJiQUNjQVV3QmpBSElBYVFCd0FIUUFRZ0FuQUNzQUp3QnNBRzhBWXdCckFFd0Fid0JuQUdjQWFRQnVBR2NBSndCZEFGc0FKd0JGQUc0QVlRQmlBR3dBWlFCVEFHTUFjZ0JwQUhBQWRBQkNBQ2NBS3dBbkFHd0Fid0JqQUdzQVRBQnZBR2NBWndCcEFHNEFad0FuQUYwQVBRQXdBRHNBSkFCaEFERUFPQUJsQURFQVd3QW5BRk1BWXdCeUFHa0FjQUIwQUVJQUp3QXJBQ2NBYkFCdkFHTUFhd0JNQUc4QVp3Qm5BR2tBYmdCbkFDY0FYUUJiQUNjQVJRQnVBR0VBWWdCc0FHVUFVd0JqQUhJQWFRQndBSFFBUWdCc0FHOEFZd0JyQUVrQWJnQjJBRzhBWXdCaEFIUUFhUUJ2QUc0QVRBQnZBR2NBWndCcEFHNEFad0FuQUYwQVBRQXdBSDBBSkFCMkFFRUFUQUE5QUZzQVF3QnZBRXdBYkFCbEFHTUFkQUJwQUU4QVRnQlRBQzRBUndCbEFFNEFSUUJ5QUdrQVF3QXVBRVFBU1FCakFGUUFhUUJQQUc0QVFRQlNBRmtBV3dCVEFIUUFjZ0JKQUU0QVJ3QXNBRk1BZVFCekFGUUFSUUJ0QUM0QVR3QkNBRW9BUlFCakFIUUFYUUJkQURvQU9nQnVBR1VBVndBb0FDa0FPd0FrQUhZQVFRQk1BQzRBUVFCa0FFUUFLQUFuQUVVQWJnQmhBR0lBYkFCbEFGTUFZd0J5QUdrQWNBQjBBRUlBSndBckFDY0FiQUJ2QUdNQWF3Qk1BRzhBWndCbkFHa0FiZ0JuQUNjQUxBQXdBQ2tBT3dBa0FGWUFRUUJNQUM0QVFRQmtBR1FBS0FBbkFFVUFiZ0JoQUdJQWJBQmxBRk1BWXdCeUFHa0FjQUIwQUVJQWJBQnZBR01BYXdCSkFHNEFkZ0J2QUdNQVlRQjBBR2tBYndCdUFFd0Fid0JuQUdjQWFRQnVBR2NBSndBc0FEQUFLUUE3QUNRQVlRQXhBRGdBWlFBeEFGc0FKd0JJQUVzQVJRQlpBRjhBVE">CyberChef</a> (Decode from base64 and remove null bytes), I discovered another string encoded in base64 FroMBASe64StRInG('aAB0AHQAcAA6AC8ALwAxADAALgAxADAALgAxADAALgA1AA==').<br>By decoding the encoded string with <a href="https://cyberchef.org/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)Remove_null_bytes()&amp;input=YUFCMEFIUUFjQUE2QUM4QUx3QXhBREFBTGdBeEFEQUFMZ0F4QURBQUxnQTFBQT09">CyberChef</a> again, we retrieve the IP address that the infected machine reached to. By appending the uri (/news.php) to the IP address, we’ll get the full URL.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2543,"sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large"><img src="https://1earnwithren.wordpress.com/wp-content/uploads/2024/10/image-29-21.png?w=485" alt="" class="wp-image-2543" /></figure>
<!-- /wp:image -->
