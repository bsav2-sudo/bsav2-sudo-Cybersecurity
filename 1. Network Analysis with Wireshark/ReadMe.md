# 1. Network Analysis with Wireshark

![wiresharkimage](https://github.com/user-attachments/assets/3b9b807c-2c52-4498-9e8e-b389d03e88d3)

### Skills Utilised

<code>Wireshark</code> <code>IPv4</code> <code>TCP</code> <code>UDP</code> <code>Traffic Analysis</code> <code>Traffic Filtering</code> <code>Problem Solving</code> <code>Packet Headers</code> <code>HMTL</code> <code>Network Scanning Techniques</code> <code>CVE Exploits</code>  

## Overview

This project looks at **3** PCAP files obtained from the Wireshark Wiki page. The project demonstrates the understanding of utilising Wireshark and also being able to identify malicious traffic from PCAP files, ranging from small PCAP files to large captured files that have been collated over a number of dates. These PCAP files emulate the scenarios in which Network forensics may be required as evidence as part of an investigation, these range from a vulnerablity being exploited to an emulated recconaisance on a target which is seen in the real world. The ability to be able to identify these as part of a Cybersecurity role is very important, as it can help make firewall and IDS rules that can stop attacks from happening and also be able to determine malicious traffic from normal user traffic.

<br>

## PCAP Analysis 

## 11-DAYS-OF-SCAN

Context: This file contains traffic information which is collated over 11 Days - **Suspected Scanning From External Host**
File Size: 29.1 MB
File Name: 2024-08-30-approximately-11-days-of-server-scans-and-probes.pcap
File Type: .pcap
Timeframe: 19/08/2024 to 30/08/2024

#### Start of Analysis

To start this analysis - the first step I took was to find out all the details of the pcap file from the "Statistics" page which is found in <code>Statistics -> Capture File Properties</code>. This allows me to be able to get a brief overview of the PCAP file to identify what type of packet capture it is. The questions I ask myself is, is it a large or small file?  How long was the packet capture taken for? What is the timeframe for this? How many packets were recieved over this time period? 

All of this is answered in the <code>Capture File Properties</code> which is shown below:

![Stats](https://github.com/user-attachments/assets/032a11c9-5348-4c79-b96f-0ca4c99c42b0)

From these statistics there are some things that become very apparent before even touching any of the packets. 

Firstly, this is a very large capture file that has been taken over a long period of time. This defines the approach that I can take towards the packet file, as it is such a long time frame I am going to be looking for network patterns over a long period as we know from the information of the file that I am looking for a suspected scan on a host. 

Secondly, I know that I will need to gain some more information from the file before going into the actual analysis of packets etc. Looking for what protocols are being used heavily is a very good way for me to see what traffic this server is experiencing. So now to do this I will look at <code>Statistics -> Protocol Hierarchy</code>. 

The hierarchy will give an overview of all the protocols used in this pcap file to see which is the most dominant protocol. Looking at the <code>Protocol Hierarchy</code> this is the result:

![Protocol Hierarchy](https://github.com/user-attachments/assets/1d0b3395-1f87-4d4f-8577-4cac3d5733ad)

As I can see from the Hierarchy; TCP (Transmission Control Protocol) is 94.1% of the total packets in this .pcap file, which is somewhat expected as I am dealing with a servers network capture but gives clarification to the fact that I should be keeping in mind that the suspected scanning could be a TCP style scan - as the scan could be underlying within the usual TCP traffic. A small note I have also made is I am dealing with the IPv4 protocol and there is no IPv6 packets which helps me narrow down what I should be looking for. 

It is very interesting to note that the ICMP makes up 4.1% of the packets in the .pcap file.  This may be to allow local network admins to be able to see if the server is up or down by doing simple ping tests or sending other ICMP messages. This could be something that can be part of the investigation to see if there is any suspicious ICMP messages being sent to the server.

Now that I have slightly filtered what I should expect to be looking for in this, there is one last step I want to undertake before looking at the file in full is the <code>Endpoints</code> and <code>Conversations</code> that are taking place in this file which is listed below:

![Endpoints](https://github.com/user-attachments/assets/f68d4e29-c55c-4eba-b592-45eb183a5add)


![Conversations](https://github.com/user-attachments/assets/5405bc39-fdac-41e5-b2de-0e0a6a8386c9)

Gathering the <code>Endpoints</code> and <code>Conversations</code> gives me a little bit more contact to the pcap file that I am working with - and by looking at this data - it is evident that there is once conversation that takes place the most which is something I may want to look at more when going into the pcap further.

>> <p>Conversation Details:</p>
>> <p>MAC A: "64-64-9B-4F-37-00"</p> 
>> <p>MAC B: "00-16-3C-CB-72-42"</p>
>> <p>Packets Sent: 381,462</p>
>> <p>Data Size: 24MB</p>

#### In-Depth Analysis

Now that I have been able to gather some basic informaation which can help me in this investigation, it is time to take an in-depth look at the toolset of Wireshark and trying to find patterns within the network capture that could lead to evidence of a network scan being in progress. From the previous statements, I know that one point of interest was the TCP conversations in the network capture as the TCP conversations make up 94.1% of the total traffic.

If this is to be a TCP scan that has taken place - then I can look at the amount of TCP RST's that have taken place over the whole capture time-period. This is important as when a TCP Scan takes place on a host - the connection is always RST to show if the port is open or closed which is then sent back to the endpoint conducting the scan. Keeping this in mind for the analysis I can look at the capture and filter it to show all the TCP RST's which is displayed below:

![ALL RST'S](https://github.com/user-attachments/assets/bb5ae5f2-33a1-427c-87e5-0a678c0bec97)

Okay, so looking at all the TCP RST's there is a large amount of RST Packets that are sent to and from the server - in fact they make up 52.3% of all the packets in the file. This is a good start as it means I am mostly looking at a TCP Scan being used. But now here comes the larger question, if it is a TCP Scan, what type is it. It could be any of the following:

- TCP Connect Scan
- TCP SYN Scan
- ACK Scan
- Fin Scan
- Null Scan
- Idle Scan

This is where I need to look more at the behaviour of the network and see what the packets sent over time looks like. To be able to see this visually I would love to see this on a graph to be able to visually see packet distribution over time. Luckily Wireshark has a very nice tool called **I/O Graphs** which will allow me to map any filters onto a graph over a specific period of time. great, so let me plot the <code>Total Packets</code> and <code>TCP RST's</code> onto a graph that has the full time period. This is what it looks like:

![Graph - TCP Error Over Time](https://github.com/user-attachments/assets/5cce79a4-0d7c-4f8d-bdfb-018fe19fa891)

This gives me a good starting basis to add more filters later and see if there is any correlation between the TCP RST's I see on the capture file and packets that suggest a TCP Scan type. It would be good to look at the file and see what IP Addresses I am working with for this analysis. From the <code>Conversations</code> statistics I know there quite a lot of conversations that are taking place on IPv4, so determining which IP may be doing the scanning is vital to find. So let me start looking for our scanner. Looking at the data - I think I have found our scanner:

![IP Address of Scanner](https://github.com/user-attachments/assets/c5eaa64e-7bcb-4a8c-bbb0-289d6835d73a)

Seeming as this is the endpoints with the largest conversation it could be possible - but I need to be able to back this up with further evidence. Seeing as though I have seen this is a TCP Scan, I'm going to try and narrow it down to one possible scan being used.

So firstly, applying the IP Address as a filter, I am getting two different packets back - RST and SYN. This is interesting as this may suggest I am looking at a TCP SYN scan in which the only replys our server will be sending to the scanning endpoint is SYN ACK or RST (telling the scanner if the port is opem or not).Seeing the screenshot below adds the evidence to this:

![SYN and RST](https://github.com/user-attachments/assets/2b0315c9-83f1-4e66-bfab-cd724f27121c)

Now in order to confirm this is a TCP SYN scan that has taken place I'm going to do two more things - one of them is going to take a closer look at some of the TCP SYN packets that have been sent to see what the packet looks like - does it have any data? Does the packet look malformed or contain interesting information. I'm going to have a look:

![SYN FLOOD](https://github.com/user-attachments/assets/3e240dd6-ffb0-4fc2-a228-249c3997be6e)

Looking at this SYN packet I can see that I am definetly looking at a SYN packet amd there doesn't seem to be anything that looks malicious or suspicious about the packets being sent - but that doesn't make me doubt this is a TCP SYN scan on this server, so the next thing I am going to do is go back to my I/O graph from earlier but plot a new line for the TCP SYN packets that are sent over the 10 day period. If there is any correlation to the TCP Errors (RST Packets) then this can add more evidence to this scan. And sure enough once I add my extra plot into the graph - there is a correlation:

![SYN over time](https://github.com/user-attachments/assets/7715009e-6819-402b-99b4-717fedfbff38)

Success! I can confirm what type of scan has occured on this server - and since reading more documentation on Wireshark - there is one more mode I can use to confirm this. <code>Expert Mode</code> can let me see what Warnings and Errors Wireshark finds within this .pcap file - so lets see what I get:

![Expert Info](https://github.com/user-attachments/assets/564860e4-a2e6-4934-b24c-4aeba67f0048)

Looking at the TCP errors and warnings that Wireshark has gave in Expert Mode - this confirms the traits of a TCP SYN Scan including the TCP RST'S, Zero Windows Segment in TCP Packets and more.

Now that I can see the scan that has been done, how would I mitgate this scan from happening in future?

#### Mitigate against a TCP SYN Scan

One of the ways to be able to mitigate against this style of scan is by setting up <code>some robust and strong filtering rules on the IDS/IPS or Firewall</code>- this will allow for the network to be able to alert or drop packets that it would suspect are those from a TCP SYN Scan. The most effective way to do this in my opinion would be to set a rule that alerts the network admin when a thershold is met of TCP SYN and RST packets have been sent and recieved from a single external IP Addresss - this is because as part of normal network traffic TCP SYN packets take place most of the time to be able to start connections - however these send SYN ACK and ACK packets when the connection is established, not RST packets in a short amount of time to show the port is closed.

Another way to be able to mitigate from this type of scan is to <code>block known malicious IP Addresses</code> - this a basic yet effective strategy as it the network will block these IP Addresses from being able to communicate over the network and so stopping scans from happening from untrusted IP Addresses.

Furthermore - one of the other possible ways to mitigate this scan from taking place is by having <code>rate-limiting implemented on the network from incoming connection requests</code>. This can be considered the "threshold" for the incoming connections to the network and limits the ammount of incoming connections allowed on the network. The reverse of this would be also adding more backlog to the server or network so that when SYN packets are sent over the network, a server is able to handle all the SYN packets it receieves.

<br>

## TCP_SYN_SCAN

Context: A TCP SYN Flood scan that has taken place on a network.
File Size: 148KB
File Name: SCAN_nmap_TCP_SYN_SCAN_EvilFingers
File Type: .pcap
Timeframe: 03/01/2009 18:58 

In this investigation, I'll be taking a closer look at a TCP SYN Flood scan that has taken place on a network - specifically looking at the SYN packets for any noticable mnaipulation and characteristics of the pcap such as timeframe of the attack and other information that point to this being a SYN Flood attack. So now I'm going to start my investigation.

Obviously this .pcap file is going to make it easy to find indicators of a SYN Flood attack as that is all the .pcap file consists of - there is no normal network traffic mixed into this pcap but is a proof of concept of being able to identify a SYN Flood attack.

The first thing I always like to do is get the basic information from the pcap file on the Statistics view. This just gives me a good starting point for my analysis and gauage an overall view of the file, is it going to be a large file? Is it a large or small timeframe? How many packets am I going to be dealing with inside this PCAP? Do I have any other information that could be useful for me in this investigation. So here is that basic information listed below:

![File Properties](https://github.com/user-attachments/assets/a670e737-ce7b-40e3-8281-b983e9b7e1f4)

So from looking at this I can already see there are a decent amount of packets I can be looking at (2020 to be exact) which means that from the context given by the pcap file - this is going to be a SYN Flood on a good amount of ports. What the info also tells me is that this packet capture lasted 1. 241 seconds, giving me another indication that this packet catpure was taken in a small timeframe yet loads of packets were sent over the network. Something to keep in mind.

Looking immediately at the PCAP file there is one thing that is very obvious:

![SYN AND RST](https://github.com/user-attachments/assets/9afcaba9-8734-416d-9e9e-e820bcc1be6f)

From the screenshot above I can see that there are two types of TCP packets that I will need to look at in this packet capture: TCP SYN and TCP RST. From the visible red and grey indicators on the pcap I can see that TCP SYN packets are being sent first from an IP of <code>192.168.1.10</code> on port <code>35503</code>. Once these SYN Packets are sent on the network to different ports on the IP Address <code>192.168.1.25</code>, This IP Address then replies with a TCP RST packet to the host IP of <code>192.168.1.10</code> to indicate that the port the SYN Packet was sent to is closed. From there the Host IP then keeps sending TCP SYN packets from port <code>35503</code> to more ports on the destination ports to ask if they are open or not.

That was a lot of words to explain what is going on so I'm going to use an image instead:

![Figure 1](https://github.com/user-attachments/assets/8cd77b71-a46b-4f11-a927-fcc453dbdbde)
>> A Comprehensive Detection Approach of Nmap: Principles, Rules and Experiments (Liao and Zhou, 2020)

From this figure I can see that I am dealing with a host scanning <code>192.168.1.25</code> to see what ports are open and which are closed, definelty as sign this is a port scan using the TCP SYN Scan method. But this proves that this is also a flood attack as all these packets are being sent over a short period of time (a quick recconaisance of the host). This is shown when plotting <code>Total Packets, SYN Packets and RST Packets</code> on my favourite I/O Graph in Wireshark:

[SYN Scan Graph.pdf](https://github.com/user-attachments/files/17894753/SYN.Scan.Graph.pdf)

This graph shows how the flood of packets has been sent over the netwok in such as short period of time.

But is there any indicators in the actual SYN Packets that are being sent that this is a port scan that is taking place on a host. Well that is where I am going to be looking into now with the help of Wireshark's Expert Mode which will give my indicators of any malicious or malformed header information. So let me pull one up now from the pcap file.

![Expert Analysis](https://github.com/user-attachments/assets/1595f861-bc1e-41b2-a8dd-5b70b87cbf45)

Looking at this there are already two things which Wireshark has found two indicators which should be of interest. The first is the SYN Flag being sent from the scan host to the host. The second indicator is that the SYN Packet within its options <code>Does Not Contain a SACK PERM Option</code>. The SACK PERM option is there so that host A is willing to recieve SYN Acknowledgment messages or informaton. This is important when looking at a SYN Flood attack as when a normal TCP Conversation is being established, a TCP SYN ACK packet is sent back to Host A to confirm the conditions of communication before sending an ACK back to Host B. Because there is no option for this, Host A doesn't want to take part in a SYN ACK from Host B and just wants to know if the connection is there or not which is typical of a SYN Scan.

What is also very interesting in the use of RST Packets from Host B rather than FIN Packets. FIN Packets are used when a conversation has came to a close and both Host A and B are happy with the conditions of this closing. RST Packets on the other hand are more abrupt and is used to tell both sides to stop communicating all together. Within this scenario, the use of RST Packets being mostly used indicates that all communications are being shut immediately after Host A finds if Host B's port is open or not.

All of these are the indicators that can be seen in network traffic to see if a TCP SYN Flood Attack has taken place on the network. It is worth keeping in mind however that this pcap is a simple one to demonstrate what a SYN Flood attack looks like - within normal network traffic this would be harder to pick out so knowing what to look for can narrow down the scope of the packet capture to only find the details needed.

#### How To Stop A SYN Flood 

One of the ways to be able to mitigate against this style of scan is by setting up <code>some robust and strong filtering rules on the IDS/IPS or Firewall</code>- this will allow for the network to be able to alert or drop packets that it would suspect are those from a TCP SYN Scan. The most effective way to do this in my opinion would be to set a rule that alerts the network admin when a thershold is met of TCP SYN and RST packets have been sent and recieved from a single external IP Addresss - this is because as part of normal network traffic TCP SYN packets take place most of the time to be able to start connections - however these send SYN ACK and ACK packets when the connection is established, not RST packets in a short amount of time to show the port is closed.

Another way to be able to mitigate from this type of scan is to <code>block known malicious IP Addresses</code> - this a basic yet effective strategy as it the network will block these IP Addresses from being able to communicate over the network and so stopping scans from happening from untrusted IP Addresses.

Furthermore - one of the other possible ways to mitigate this scan from taking place is by having <code>rate-limiting implemented on the network from incoming connection requests</code>. This can be considered the "threshold" for the incoming connections to the network and limits the ammount of incoming connections allowed on the network. The reverse of this would be also adding more backlog to the server or network so that when SYN packets are sent over the network, a server is able to handle all the SYN packets it receieves.

<br>

## EXPLOIT_METASPLOIT_CVE2012_1723


Context: This .pcap file looks at network traffic taken from a computer that has been attacked by the CVE2012_1723 attack.
File Size: 116KB
File Name: EXPLOIT_metasploit-cve-2012-1723_EmergingThreats
File Type: .pcap
Timeframe: Unkmown 

[NIST CVE2012-1723](https://nvd.nist.gov/vuln/detail/CVE-2012-1723)

Description of CVE2012-1723:

>> Unspecified vulnerability in the Java Runtime Environment (JRE) component in Oracle Java SE 7 update 4 and earlier, 6 update 32 and earlier, 5 update 35 and earlier, and 1.4.2_37 and earlier allows remote attackers to affect confidentiality, integrity, and availability via unknown vectors related to Hotspot.

Having context behind the attack that has taken place on the network is great to have - it very much narrows down the scope of what I should be looking for in this packet capture. For starters this is an attack on the Java Runtime Environment - to be truthful I have limited knwoledge on Java - however knowing that I am looking for something to do with the Java Runtime. Looking online, there are some files that are associated with Java that I could potentially be looking out for, Java Scripts, Java Executbles perhaps? All a great starting point in what I should be looking for.

From the description of this vulnerability provided from NIST - I can also see that this vulnerability can compromise the CIA triad in Cybersecurity which means that this vulnerability is a threat to protecting data.

So I'm going to get started, yet again the first thing I am going to do is look at the general information of this packet capture file - for me always a good place to start before diving straight into the packet capture file. One nice thing looking at this pcap file is I am going to be looking at a small data set of 190 packets, makes a nice change from the large 27MB file earlier in this investigation:

![General Info](https://github.com/user-attachments/assets/9d6a3a15-4024-46bf-9d73-c37180cfe428)

Okay, so now I have had a look at the basic data - I'm going to look at the protocol hierarchy to see which protocol is mostly being utilised in this pcap, so let's have a look:

![Protocol Hierarchy](https://github.com/user-attachments/assets/e6300932-21a9-41a1-a967-5d3a0f6be16a)

Okay so I have a lot of TCP conversations happening in this pcap, but most interesting of all there is some HTTP packets being sent which will be for any Java files being sent over the network. So the first thing I'm going to do is look at the <code>HTTP Streams</code> in this pcap. The <code>HTTP Stream Follow</code> allows me to see the HTTP conversation between the client and the server and could give me some good clues from the attack. So from the HTTP Stream this is what I found:


![HTTP Stream 1](https://github.com/user-attachments/assets/5c679d81-4dc9-4ee0-806c-7321211d25aa)
![HTTP Stream 2](https://github.com/user-attachments/assets/bbd82ef3-2a88-477e-b287-544a91044728)
![HTTP Stream 3](https://github.com/user-attachments/assets/2503ca01-9c25-4b68-bbfd-fc08fe45c2c1)

Looking at all the HTTP Streams in this packet capture - I can see the entire HTTP Conversation. One of the interetsing things I noticed within these streams is that there is a HTTP GET request for a .jar file. Like I said before I don't know much about Java however doing some research finds that the .jar file is a zipped version of Java file to reduce the file size. There may be some malicious files kept in this JAR file. So in a closed environmet I'm going to get all the HTML Objects from this pcap. Nicely, Wireshark has a feature called <code>Export HTML Objects</code> which will locally download all the HTML Objects in the pcap. So let me download them:

![HTML Obejcts Picture](https://github.com/user-attachments/assets/fa89182d-79a1-42bf-a2df-b1ec48848d4b)

Just as I thought, theres what I've been looking for:

![Java Executable](https://github.com/user-attachments/assets/dc46971c-48c1-41dc-8ab4-65898cdfb0a0)

So I need to "Unzip" this and see what the contents are of this JAR file. Now there are many tools to do this including using the command line to get Java to "Unzip" the contents using the following command:

<code>C:\Java> jar xf myFile.jar</code>

And here are the results of that execution:

![JAR Unzipped Picture](https://github.com/user-attachments/assets/33c63570-cc6c-4c14-acea-36aaee827cb7)

MSF? As in the Metasploit Framework? Okay this will be interesting to look at as this may contain the payload for this attack. Just as I suspected...

![Payload](https://github.com/user-attachments/assets/09b745cf-9e19-4023-92d1-11bbc1fb23bd)

Now being the curious person I am - I really want to see what this payload is and what it is doing. This for this project goes a bit out of scope as this project only focuses on the network analysis on the Exploit and how it got there in the first place which I have now been able to identify. 

#### Mitigate against CVE2012_1723

Because this is a known vulnerability - it would be **highly** reccomended to patch and update Java software as soon as it becomes publicly available as this will patch this exploit. Reading the Relase Notes for the next update/patch would be able to identify that this vulnerability has been highlighted and fixed. It would also be recommended to block the JAR file from being downloaded or executed - this could come in the form of ebing added to a blacklist at Network level so that if the .jar file is detected on the network it can be removed or at a Endpoint level where an Agent such as Arctic Wolf's <code>Arctic Wolf Agent</code> would see this jar file and remove it from the users device before it could be executed.

<br>

#### Update 23-11-24

Since this blog - I have been able to see the decomplied code of the JAR file using a Java Decompiler and found there to be 4 .class files contained in this which include a MetaSploit payload file. Now as mentioned in the blog I have limited knowledge on the Java Language so I am unable to identify what is occuring in these .class files, however I wanted to upload these the blog to see what is wrote in the files.

They are linked below :)
Files are in a .txt format to be able to be read only.

[PayloadX](https://github.com/user-attachments/files/17882712/PayloadX.txt)

[Attacker](https://github.com/user-attachments/files/17882713/Attacker.txt)

[Confuser](https://github.com/user-attachments/files/17882714/Confuser.txt)

[ConfusingClassLoader](https://github.com/user-attachments/files/17882715/ConfusingClassLoader.txt)

## Outcomes of Investigation

From doing these Network Analysis investigations - there is a few deductions that I have made about Network Analysis in the field of Cybersecurity:

- Being able to identify scans and attacks in a large Network Capture file is very useful and a good skill to have - using effective filters that pro-actively find indicators of a scan or attack make it easier from an Analyst perspective to take an investigation further and commit the necessary actions.
- Once a scan or attack has taken place, enforcing rules such as effective IDS/IPS and Firewall rules is a pro-active way to prevent similar scans and attacks from happening. Making said rules well strcutured and up to date is a really good way to have an effective Network defence. For example having an IDS/IPS rule that alerts an Analyst of malformed packets being sent over the network.
- Keeping up to date with the latest hacker techniques for Network scans and attacks also makes it good to stay ahead of the game - for example implementing a good DDos protection system that can be adapted for different types of DDos Attacks.
- The ability to test a network using tools such as Scapy in a closed environment can help test the Network Security measures to find vulnerabilities that need to be addressed or re-modelled - for example sending malformed packets on the netowrk to see if any alerts or warnings are correctly triggered.
- Using Endpoint Security such as Agents can help with files that are known to be malicious from being executed on an endpoint - as files often don't come from just one individual website and are distributed across many websites.













