# Download From Fake Software Website

![site-logo-01](https://github.com/user-attachments/assets/03aab929-d5ef-4517-9c24-2f32ee7927e9)

## Overview

This project looks at putting my Network Analysis skills to the test with a PCAP file from the site Malware-Traffic-Analysis. The website gives practice PCAP files to practice network analysis skills on and provide realistic scenarios. I really like using this tool as there is a whole range of scenarios to choose from and test my skills on. The scenario for this analysis is as follows:

_"You work as an analyst at a Security Operation Center (SOC). Someone contacts your team to report a coworker has downloaded a suspicious file after searching for Google Authenticator. The caller provides some information similar to social media posts at:_

_https://www.linkedin.com/posts/unit42_2025-01-22-wednesday-a-malicious-ad-led-activity-7288213662329192450-ky3V/
https://x.com/Unit42_Intel/status/1882448037030584611_
_Based on the caller's initial information, you confirm there was an infection.  You retrieve a packet capture (pcap) of the associated traffic.  Reviewing the traffic, you find several indicators matching details from a Github page referenced in the above social media posts.  After confirming an infection happened, you begin writing an incident report."_

Here is also a link to the scenario if you want to go and have a look - there is more great training content available on the site so definetly one to have a look at! Link to Scenario: [Link](https://www.malware-traffic-analysis.net/2025/01/22/index.html) now without further ado - lets get cracking

## PCAP Analysis 

For this analysis - I wanted to use something over than Wireshark to get a new experience of working with a different workspace - I decided in this case to use Zui with the Suricata detection rules added - for this challange I had heard about the program with Suricata rules and wanted to see if it made life any easier when finding the answers.

For this task there are several questions that need to be answered:

<img width="470" alt="Tasks" src="https://github.com/user-attachments/assets/6281c4c5-7e27-4b42-8ac9-6e4863126de4" />

Okay - lets start at the very beginning (a very good place to start). So we are looking for the IP Address of the infected Windows client - I think the best place to start looking for this will be through the smb_mapping packets - jsut as this might give some details into the user as well so we can tick two boxes in one. Going into Zui and entering the search term __path="smb-mapping" brings me all of these packets and...

<img width="1439" alt="IP Address" src="https://github.com/user-attachments/assets/d9d07e27-fc8a-4f1a-a1c5-24c8573f8a7b" />

Okay I can clearly see one IP that also had a host name on it so that gives us two of the answers:

IP Addr == 10.1.17.215
Host Name == WIN-GSH54QLW48D

I can confirm the host name by just expanding the tab and seeing the name...

<img width="953" alt="Host Name" src="https://github.com/user-attachments/assets/f3f3e13e-4dac-4f51-b262-995cf76b6a63" />

Yep this looks like our host - cool we've got two in one there - so lets go to the next question - what is the MAC address of it. Well within Zui I can see that there are some DHCP packets flying about - so maybe one of these will give us an answer if we can find our IP Address.

<img width="958" alt="MAC Address" src="https://github.com/user-attachments/assets/6979ddbf-0653-4a64-8509-90a568efb271" />

Yep we can see here our IP address was given by 10.1.17.2 - and within the fields is the MAC Address - another nice easy win there so lets get that down:

MAC Address == 00-d0-b7-26-4a-74

Okay next is the user - now I think a good place to start would be to look at the kerberos packets and just see what information appears on there - if I am lucky there may be some LDAP's on there that could just give us the user's name.

<img width="956" alt="User Account" src="https://github.com/user-attachments/assets/53bd1b2b-de37-4a1a-b9df-d4e5bf943757" />

Yep there we go as I can see here we have a few LDAP's which give back a username and match the domain - I'm quite confident this is going to be our user so lets note it down:

User == shutchenson

