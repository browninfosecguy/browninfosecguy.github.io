---
layout: post
title: "Ransomware Guide"
twitter_image: /images/Ransomware-Guide/Diagram2.png
description: "Ransomware Guide."
date: 2020-10-07
feature_image: /images/Ransomware-Guide/Diagram2.png
tags: [CyberSecurity]
hashtags: CyberSecurity,Ransomware
---
According to [Kaspersky](https://www.kaspersky.com/resource-center/definitions/what-is-ransomware) 
>Ransomware is malicious software that infects your computer and displays messages demanding a fee to be paid for your system to work again.

Ransomware is a major threat for any organization in current cybersecurity landscape. Ransomware can cripple the ability of an organization to fulfill their business obligations and can potentially shutter the business for good. Ransomware groups operate like any other mature organization with specific departments responsible for specific responsibilities. Ransomware groups have dedicated teams responsible for ransomware development, ransomware delivery, handling communication with victims etc. Some of the most notorious and active ransomware groups are[^1]

+ RobbinHood ransomware
+ NetWalker ransomware
+ NetWalker ransomware
+ Maze ransomware
+ REvil ransomware
<!--more-->

Government agencies like FBI, CERT, NIST regularly release guidance on protecting against ransomware attacks. Most recently The Cybersecurity and Infrastructure Security Agency (CISA) released the Ransomware Guide[^2]. 

While these guidelines provide a holistic approach to protect against ransomware most organizations struggle to follow and implement all the controls recommended in these guides. In this blog post I try to highlight some critical security controls an organization can implement to protect them from ransomware attack.

### 3-2-1 Backup Rule
The NIST Special Publication 800-53 recommends regular backups under the contingency planning (CP) control. Backups are the most important security control when it comes to recovery from a ransomware attack. Most organization however either don't have a backup strategy in place or implement backups without following the 3-2-1 rule. The 3-2-1 backup rule states that:

***3 – One should make 3 backup copies.***

***2 – Store backups on 2 different type of media.***

***1 – Store 1 copy of backup at an offsite location.***

{% include diagram.html imageurl="/images/Ransomware-Guide/Diagram1.png" caption="3-2-1 Backup rule" %}

The offsite location should be both physically and logically segmented from your environment. Most organization use an offsite cloud-based backup solution however they do not segment this from their IT environment which results in the backups also being encrypted during a ransomware attack.

>*Tip: If you have Office 365 subscription instruct your employees to use OneDrive for storing critical documents. OneDrive notifies and restore files from a ransomware attack[^3]*

If you can't follow 3-2-1 backup rule, then at a minimum order external hard drive from amazon and take backup of your critical systems.

### Implement Multi-Factor Authentication (MFA)
MFA if implemented properly can protect an organization from number of attacks including ransomware. MFA is single most effective security control an organization can implement to considerably reduce their cybersecurity risk. 

The world we live in today, most of the workforce is working from their home as such there is no excuse to not implement MFA. According to Microsoft[^4] 99.9% of attacks on an account can be prevented using MFA.

### No RDP over the Internet
According to recent report from coveware, RDP is the number one attack vector for ransomware followed by Phishing.
{% include diagram.html imageurl="/images/Ransomware-Guide/Diagram4.png" caption="Source: Coveware" %}
Despite this most organization still enable RDP over the internet. A Simple Shodan search for Canada reveals 31,628 exposed RDP instances. 
{% include diagram.html imageurl="/images/Ransomware-Guide/Diagram5.png" caption="Exposed RDP Ports for Canada" %}

### Enable Microsoft Application Guard
Phishing emails often use Microsoft office documents such as the one shown below to social engineer their way inside an organization. Macros embedded in office documents launch the first stage of the attack.

{% include diagram.html imageurl="/images/Ransomware-Guide/Diagram3.png" caption="Source: https://twitter.com/Unit42_Intel/status/1313545578803011595?s=20" %}

Microsoft recently released Application Guard[^5] for Office 365 publicly. According to Microsoft 
>Microsoft Defender Application Guard (Application Guard) is designed to help prevent old and newly emerging attacks to help keep employees productive. Using our unique hardware isolation approach, our goal is to destroy the playbook that attackers use by making current attack methods obsolete.

Application guard should be enabled for office. Application guard act as a container for office documents. The technology is based on sandbox using hardware-based virtualization. When a user opens a document its opened in this container thus protecting the underlying operating system. 

### Implement Raccine

Raccine is a tool developed by Florian Roth (<https://twitter.com/cyb3rops>). Raccine acts as a vaccine against ransomware. A ransomware starts by firt deleting the shadow copy backups before encrypting the files and folders. Shadow copies are based on file changes stacked on top of each other. These shadow copy backups are used to create restore points. If you want to read more about shadow copies please refer to Microsoft Documentation[^6].

{% include diagram.html imageurl="/images/Ransomware-Guide/Diagram6.png" caption="Source: https://github.com/Neo23x0/Raccine" %}


In order to delete the shadow copies ransomware uses a system utility called ***vssadmin***. The Raccine tool essentially works by killing the parent process launching vssadmin hence killing the ransomware process. The tool is still in early stages and now involve killing explorer process itself, however that might change in the future. More details about the tool can be found here <https://github.com/Neo23x0/Raccine>

Oh, did I mention its free!!

### Scan and Patch

Most ransomware families relies on exploiting unpatched systems. It is imperative for an organization to regularly scan internal and external environment and regularly patch. If you don't have the budget to run external scans leverage Shodan. One can easily discover the ports and services exposed externally using Shodan. If your organization has web presence use free tools such as:

1. <https://sitecheck.sucuri.net/>

2. <https://www.immuniweb.com/websec/>

Internal scans can be accomplished by buying something like Nessus. Nessus is not super expensive and does a good job with scanning.

The idea here is to know about your weakness and fix them aka patching.

If you don't patch, you are doomed to become victim of ransomware attack. If you can't patch because of dependencies, they you should:

1. Start finding replacement for the software preventing implementing a patch and
2. Implement compensatory controls to minimize the risk from not patching.

{% include thanks.html %}

### References:

[^1]: <https://www.microsoft.com/security/blog/2020/04/28/ransomware-groups-continue-to-target-healthcare-critical-services-heres-how-to-reduce-risk/>

[^2]: <https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf>

[^3]: <https://support.microsoft.com/en-us/office/ransomware-detection-and-recovering-your-files-0d90ec50-6bfd-40f4-acc7-b8c12c73637f>

[^4]: <https://www.microsoft.com/security/blog/2019/08/20/one-simple-action-you-can-take-to-prevent-99-9-percent-of-account-attacks/>

[^5]: <https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/install-app-guard?view=o365-worldwide>

[^6]: <https://docs.microsoft.com/en-us/windows-server/storage/file-server/volume-shadow-copy-service>
