---
layout: post
title: "Active Directory Lab Setup Tool"
twitter_image: /images/ActiveDirectory-Lab/Diagram1.png
description: "A neat use case of using Out-GridView with PassThru."
date: 2020-11-29
feature_image: /images/ActiveDirectory-Lab/Diagram1.png
tags: [CyberSecurity]
hashtags: PowerShell,PenTest,RedTeam
---
The AD Pentesting tool is a tool created in PowerShell to quickly setup an Active directory lab for testing purposes. This tool can help setup a Domain controller and Workstation in a lab environment quickly and effectively. While the tool is specifically written to configure an Active directory environment in a lab environment the tool can be easily stretched for production environment as its released under MIT license.
<!--more-->

The process to manually configure a domain controller using GUI can be painful especially if you need to create and teardown the lab frequently. This single tool can not only configure a domain controller quickly but can also automate additional configuration such as creating share, creating users, and configuring group policy object for disabling Windows Defender which is something desirable especially in a lab environment. 

This blog post will provide an overview of the tool and demonstrate how it can be used to automate configuring a domain controller and workstation. 

The tool is a PowerShell script called “ADPentestLab.ps1” and is available on [GitHub](https://github.com/browninfosecguy/ADLab) under MIT License.

There are few things to keep in mind before using the tool.

1. This tool should be run as an Administrator. 
2.	The tool takes the liberty to configure passwords where required without requiring user input. The password we configure is “Password1”, this can be easily changed if required.
3. The tool was tested on a fresh install of Windows 2019 Server and Windows 10 Enterprise workstation.

When we run the tool, user is presented with following options:

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram1.png" caption="Figure 1: Basic Options of the Tool" %}

As shown at ①, Option 1-5 are applicable to Server except option 3 which is applicable to both Server and workstation. Option 6 and 7 are exclusively for configuring workstation. 
Once you are done installing a Windows Server and Windows 10 Enterprise, you can use this tool for configuration.
Figure 2 shows a fresh Windows 2019 server install.
 
{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram2.png" caption="Figure 2: Windows 2019 Server" %}

We begin by configuring the Domain Controller as shown in Figure 3. Option 1 allows us to assign our computer a new name and configure a static IP address. Although, not required it is always a good practice to name the computer to a more user-friendly name.

Important thing to note, when we display information about network interfaces, one can observe our machine has three interfaces, well one is loopback. Our lab set up has a virtual network using subnet 192.168.25.0/24 the other IP address 10.0.3.15 is from the NAT interface. Why is this important? because we need to configure our static IP address to correct interface. The displayed information will help us determine the correct Interface Index which we need to provide at ③. 
Also important is the Prefix length as shown at ⑤, we need to enter the length and not the subnet mask. Once we have provided the necessary configuration details the computer will be restarted. 

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram3.png" caption="Figure 3: Configuring Machine name and static IP" %}

If everything went according to plan your server should reflect something like our lab server as shown in Figure 4.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram4.png" caption="Figure 4: New machine name and static IP" %}

The next step is to install the role of Active Directory Domain Services on the server and configure the Domain controller. As shown in Figure 5, Option 2 installs the role of Active Directory Domain Services and configure the Domain controller after we supply the forest name. 

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram5.png" caption="Figure 5: Setting up Domain controller" %}

Once the role is successfully installed the setup for Domain controller begins as shown in Figure 6.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram6.png" caption="Figure 6: Finalizing Domain controller setup" %}

Once the Domain controller configuration finished the machine restarts and if everything went according to plan our server will be configured as shown in Figure 7.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram7.png" caption="Figure 7: Configured Machine name and Domain" %}

The next step is to configure users, create a GPO to disable windows defender and create a share. Option 3,4 and 5 accomplishes that for us. We can clearly see our user accounts created correctly in Figure 8. 

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram8.png" caption="Figure 8: User accounts" %}

Figure 9 shows user John Conner added to Domain admins group.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram9.png" caption="Figure 9: John Conner part of Domain Admins" %}

Figure 10 shows the GPO to disable Windows Defender also Figure 11 shows the network share named “hackMe” configured correctly.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram10.png" caption="Figure 10: GPO to disable Windows Defender" %}

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram11.png" caption="Figure 11: hackMe Share" %}

Now that our Domain controller is configured and ready. The next step is to configure our workstation and join the Domain.  We start with a fresh install of Windows 10 Enterprise as sown in Figure 12.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram12.png" caption="Figure 12: Workstation Name" %}

When we try to run the tool on a fresh install, we get the error as shown in Figure 13. We can easily fix it by changing the script execution policy.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram13.png" caption="Figure 13: Configuring Execution Policy" %}

Once the execution policy is changed, our tool start working again. To add the workstation to the domain we need to configure the domain name server on our workstation. The domain name server should point to the IP address of our Domain controller. We start by selecting option 6 as shown at ①, we are then displayed with network interfaces configured on the machine. As discussed earlier we need to select the correct network interface to configure the DNS server. On our lab server we select interface 5 as shown at ③ and then providing the IP address of our Domain controller as shown at ④. Once we are finished the machine restarts.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram14.png" caption="Figure 14: Configuring DNS" %}

We can confirm that the DNS is configured correctly by observing the properties of network interface as shown in figure.

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram15.png" caption="Figure 15: Configured DNS" %}

The only thing left to do is add our workstation to the Domain. Option 7 help us add the workstation to the Domain, we need to provide the domain name as shown at ② we are then prompted for Domain Admin credentials. The system will restart itself and if everything went according to plan the workstation is added to the Domain as shown in Figure 

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram16.png" caption="Figure 16: Adding workstation to Domain" %}

{% include diagram.html imageurl="/images/ActiveDirectory-Lab/Diagram17.png" caption="Figure 17: Workstation is added to the Domain" %}

I hope this tool can help automate the process of confguring an Active Directory lab for your testing environment quickly and effectively. If you have any comments or suggestions please feel free to contact me.

{% include thanks.html %}