---
layout: post
title: "Ransomware Tabletop Exercise"
twitter_image: /images/Ransomware-Tabletop-Exercise/Diagram1.png
description: "Ransomware Tabletop Exercise"
date: 2020-10-20
feature_image: /images/Ransomware-Tabletop-Exercise/Diagram1.png
tags: [CyberSecurity]
hashtags: CyberSecurity,Ransomware,DFIR,InfoSec
---
>"Everybody has a plan until they get punched in the mouth" - Mike Tyson

CyberSecurity Version

>"Everybody has an "Incident Response plan" until they get hit by a Ransomware" - Sonny

A well documented and well rehearsed Incident Response (IR) plan can help an organization when dealing with a security incident such as a Ransomware attack. Often an IR plan can be the difference between an organization surviving the cyber attack or going out of business[^1]. 

The key to have a good IR plan is to regularly conduct tabletop exercise and address the gaps discovered during the exercise. Ransomware has wreaked havoc in recent times as such an IR plan is incomplete if it does not include the scenario addressing ransomware attack. This blog post discusses some key questions which should be part of an IR tabletop exercise for ransomware attack scenario.
<!--more-->
Following are three main scenarios after an organization is hit with a ransomware attack
>
>1. An organization follows the 3-2-1 backup rule[^2] and has access to offsite backups and can rebuild their environment
>2. An organization does not follow the 3-2-1 backup rule but has backups, however those backups were also encrypted by the ransomware. Moreover, the organization has financial means to pay ransomware.
>3. An organization has no backups and financial resources to pay ransomware as such a rebuild is inevitable.

The flowchart below shows a typical thought process when an organization is dealing with a ransomware attack.
{% include diagram.html imageurl="/images/Ransomware-Tabletop-Exercise/Diagram2.png" caption="Ransomware decision flowchart" %}

Offsite backups clearly play crucial role recovering from a ransomware attack. However, recent high-profile ransomware attacks have shown that most organizations don't follow the 3-2-1 backup rule and end up with encrypted backups.

### Scenario 1: Offsite backups are available

If an organization follows 3-2-1 Backup rule, then they can easily recover from a ransomware attack. However, the recovery point objective (RPO) and recovery time objective (RTO) are key. Following key questions should be considered during a tabletop exercise for this scenario:

+ How long does it take to get offsite backups on premise to start the recovery process?
+ Is the contact information and process to obtain offsite backups documented and available in paper format?
+ How often do we test offsite backups?
+ How long does it take to restore systems from backup?
+ Have we restored a service in the past from offsite backups?

{% include diagram.html imageurl="/images/Ransomware-Tabletop-Exercise/Diagram3.png" caption="Recovery Process" %}
### Scenario 2: Ransomware Negotiation and Payment

If the organization decides toto pay the ransomware the negotiation process should start. It is very important that senior management is promptly available during this process as it's up to the senior management to make the important decision of agreeing on the final dollar figure for ransomware payment. During the tabletop exercise some key questions to consider are

+ Do we have internal/external ransomware negotiator?
+ How much Ransomware can we pay?
+ Does our Cybersecurity insurance cover part of the Ransomware payment?
+ How do we buy Bitcoin or other crypto money?
+ How do we transfer the Bitcoin?

Once the payment is made the Ransomware group will provide you with the decryption keys. The decryption process can take a long time moreover after decryption data should be checked for integrity. Some key things to consider
are:
+ Do we have a process to verify integrity of data once recovered?
+ How do we verify that there were no backdoors installed?
+ Do we have access to YARA scanner such as Thor[^3]


### Scenario 3: Rebuild the IT infrastructure

If an organization decides to not pay the ransomware and have no backups available, then they are left with no choice but to rebuild their IT infrastructure. The rebuilding process is an excellent opportunity to include security from the beginning. An organization should follow hardening guides such as centre of internet security benchmarks[^4] when rebuilding IT infrastructure. While rebuilding IT infrastructure is another tabletop exercise itself following should be considered if this scenario occurs:

+ What would be the operational impact?
+ What would be the financial impact?
+ Do we have to incur any contractual penalties while we are not able to provide services?
+ Do we have to incur any regulatory fines?
+ Do we have access to contractors to rebuild the environment? 


{% include thanks.html %}

### Resources

[^1]: https://cybersecurityventures.com/60-percent-of-small-companies-close-within-6-months-of-being-hacked/
[^2]: https://securityboulevard.com/2020/05/3-2-1-backup-rule-the-rule-of-thumb-to-solve-your-data-loss-problems/
[^3]: https://www.nextron-systems.com/thor-lite/
[^4]: https://www.cisecurity.org/cis-benchmarks/
