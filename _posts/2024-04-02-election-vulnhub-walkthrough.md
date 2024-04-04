---
description: >-
  Walkthrough for the Election boot2root machine on VulnHub
title: "Election VulnHub Walkthrough"
categories: [Walkthrough]
tags: [walkthrough, ctf, vulnhub]
date: 2024-04-02 20:00:00 -0500
---
This is a walkthrough for the [Election](https://www.vulnhub.com/entry/election-1,503/) boot2root machine on VulnHub.  
## Enumeration
Ran nmap to enumerate services running on the target machine  
`sudo nmap -A -p- 10.0.3.46`
>Starting Nmap 7.92 ( https://nmap.org ) at 2023-03-13 17:06 EDT  
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers  
Nmap scan report for 10.0.3.46  
Host is up (0.00057s latency).                                              
Not shown: 65533 closed tcp ports (reset)                                     
PORT   STATE SERVICE VERSION                                                  
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:                                                                 
|   2048 20:d1:ed:84:cc:68:a5:a7:86:f0:da:b8:92:3f:d9:67 (RSA)                  
|   256 78:89:b3:a2:75:12:76:92:2a:f9:8d:27:c1:08:a7:b9 (ECDSA)                  
|_  256 b8:f4:d6:61:cf:16:90:c5:07:18:99:b0:7c:70:fd:c0 (ED25519)                
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))                              
|_http-title: Apache2 Ubuntu Default Page: It works                                
|_http-server-header: Apache/2.4.29 (Ubuntu)                                       
MAC Address: 08:00:27:9C:15:00 (Oracle VirtualBox virtual NIC)                     
Device type: general purpose                                                       
Running: Linux 4.X|5.X                                                               
OS CPE: cpe:/o:linux:linux\_kernel:4 cpe:/o:linux:linux\_kernel:5                      
OS details: Linux 4.15 - 5.6                                                         
Network Distance: 1 hop                                                               
Service Info: OS: Linux; CPE: cpe:/o:linux:linux\_kernel  

Bruteforced directories using dirb, discovered an interesting logs folder...  
`sudo dirb http://10.0.3.46 /usr/share/wordlists/dirb/big.txt -w`
>[sudo] password for kali:  
>  
-----------------  
DIRB v2.22    
By The Dark Raver  
-----------------  
>  
START_TIME: Mon Mar 13 19:33:40 2023  
URL_BASE: http://10.0.3.46/  
WORDLIST_FILES: /usr/share/wordlists/dirb/big.txt  
OPTION: Not Stopping on warning messages  
>  
-----------------  
>  
GENERATED WORDS: 20458                                                         
>  
---- Scanning URL: http://10.0.3.46/ ----  
==> DIRECTORY: http://10.0.3.46/election/                                                
==> DIRECTORY: http://10.0.3.46/javascript/                                              
==> DIRECTORY: http://10.0.3.46/phpmyadmin/                                              
\+ http://10.0.3.46/robots.txt (CODE:200|SIZE:30)                                         
\+ http://10.0.3.46/server-status (CODE:403|SIZE:274)                                     
>                                                                                         
---- Entering directory: http://10.0.3.46/election/ ----  
==> DIRECTORY: http://10.0.3.46/election/admin/                                          
[...]                                                                                         
---- Entering directory: http://10.0.3.46/election/admin/ ----  
[...]  
**==> DIRECTORY: http://10.0.3.46/election/admin/logs/**                                     
==> DIRECTORY: http://10.0.3.46/election/admin/plugins/                                  

The logs folder has a file named "system.log" which, upon inspection, contains credentials for a user named "love"  
![system.log file content](images/election_systemlog.png)  

## Exploitation
Used the love:P@$$w0rd@123 credentials to start an SSH session as love on the target machine
![SSH foothold](images/election_sshfoothold.png)  

### user.txt flag
![user flag](images/election_flag_user.png)  

## Privilege Escalation
During post-exploitation enumeration, I discovered that a Serv-U FTP server is running and listening locally on port 43958  
![Serv-U FTP listening](images/election_listening_servu.png)  

By checking the Serv-U FTP logs, was able to determine that the Serv-U FTP server version is 15.1.6.25  
![Serv-U FTP version](images/election_servu_version.png)  

Ran `searchsploit serv-u 15.1` and discovered that the currently installed version of Serv-U is vulnerable to a local privilege escalation vulnerability!  
![Exploit-DB Serv-U results](images/election_servu_exploitdb.png)  

Used Exploit-DB ID 47009 to exploit the privesc vulnerability  
![Exploit-DB ID 47009](images/election_servu_exploitcode.png)  

In love's SSH session, copy/pasted the exploit code into a file named exploit.c, compiled the code, and ran the exploit  
![Serv-U FTP exploit code compilation and execution](images/election_privesc.png)  

Got root!

### root.txt flag
![root.txt flag](images/election_flag_root.png)
