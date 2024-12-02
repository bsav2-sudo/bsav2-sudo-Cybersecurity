# 6. Further PCAP Analysis with Wireshark

![wiresharkimage](https://github.com/user-attachments/assets/3b9b807c-2c52-4498-9e8e-b389d03e88d3)

### Skills Utilised

<code>Wireshark</code> <code>IPv4</code> <code>TCP</code> <code>UDP</code> <code>Traffic Analysis</code> <code>Traffic Filtering</code> <code>Problem Solving</code> <code>Packet Headers</code>  <code>Network Scanning Techniques</code> <code>ARP Storm</code> <code>802.11 Protocol</code> <code>Fragmentation</code>

## Overview

This project looks at **3** more PCAP files that I have sourced from the internet to further explore Network Traffic analysis on different attacks. This project looks at ARP Storms, IPv4 Fragmentation and also analysis of 802.11 protocol to see how it works within Wireshark. The PCAP files have been sourced from trusted websites who provide practice .pcap files to be able to practice and identify network traffic. I wanted to explore more of the practice PCAP files to be able to further my ability and being able to identify types of network attacks. I am particularly interested in the analysis of the 802.11 protocol - as for most of my learning - I have been looking at Ethernet protocols, but wireless networks are prone to attacks as well including de-authentication attacks and more. So lets get started by looking at an ARP Storm Broadcast.

## PCAP Analysis 

## ARP-STORM
<p>Context: This file contains traffic information from what is a suspected ARP Storm</p>
<p></p>File Size: 46.1 KB</p>
<p>File Name: arp-storm.pcap</p>
<p>File Type: .pcap</p>
<p>Time: 2004-10-05 / 15.01.05</p>

#### Start of Analysis

This pcap file looks at what is believed to be an ARP Storm Broadcast that has taken place on a network. Looking at an ARP Broadcast Storm is not necessarily a type of attack but from a network analysis standpoint it can help me identify what one looks like on a network and be able to deduct it as a storm taking place on a LAN. The description of a ARP Broadcast Storm is as follows:

"ARP-based Distributed Denial of Service (DDoS) attacks due to ARP-storms can happen in local area net- works where many computer systems are infected by worms such as Code Red or by DDoS agents. In ARP. attack, the DDoS agents constantly send a barrage of ARP requests to the gateway, or to a victim computer." - Kumar and Gomez, 2010.

So from this definition I already know what to expect within this pcap - ARP Packets being sent on the network which are all ARP requests to the local default gateway. So first I'm going to open up the pcap file in Wireshark and lets see what we have:

![Overview](https://github.com/user-attachments/assets/059fbece-4bdc-46eb-9aa1-ef3caec9dd03)

And staright off the bat I can see that I am dealing with ARP Packets that are all ARP request packets. Just looking at the pcap from a far - there is one Source for all of these ARP Packets which is "Cisco_af;f4;54". So now I'm going to look at how many of these packets have been sent in this time period - both looking at the Statistics of the PCAP and also plotting the ARP Packets on a graph against time to see how the Storm has taken place:

![Statistics](https://github.com/user-attachments/assets/571b0027-c527-47be-98a0-515b85c9d83f)

![ARP STORM Graph-1](https://github.com/user-attachments/assets/44d7a78c-8636-48f1-8874-d808c6640d5d)

The graph is very basic but it tells me everything I need to know about it to confirm what I am looking at is an ARP storm - it is a lot of ARP Packets being sent in a short amount of time which is the basic definition of an ARP Storm. Now that I can confirm this is an ARP Storm - I want to take a closer look at an individual ARP Packet from this pcap just to see what it looks like:

![Individual Packet](https://github.com/user-attachments/assets/2d37348e-d46f-44d2-8f7f-79407bd4663f)

Looking at one of the individual packets - there is some interesting parameters that are set within it which are worth knowing for me:

- Ethernet Padding - usually within an Ethernet packet there will be a padding made up of 0's added if the packet doesn't meet the minimum size of 64 bytes - however from looking at this packet I can see that Wireshark has noted there is no padding but there may be padding of non 0's which is something interesting to note (see screenshot below for this)

![Ethernet Padding](https://github.com/user-attachments/assets/de467965-26e4-4ff0-9841-e1677dc15393)

- Source of ARP Packets - the source for all of these ARP packets is coming from a Cisco Device with the MAC Address of 00;07;0d;af;f4;54 - that comes from the Layer 3 address of 69.76.216.1. A strange IP Address as it is not within the Private Address range - may be something worth looking at later.

![CISCO Device](https://github.com/user-attachments/assets/8eb23c04-8900-4d64-a9d7-66bf33be1bf5)

#### Mitigation against ARP Storm

To stop an ARP Broadcast Storm from happening on the network - the following steps can be taken:

- Identify the source of the Broadcast Storm by using a tool such as Wireshark or another network analysis tool - from there you can identify the source of the storm and change what needs to be changed.
- Double check for duplicate IP Addresses on the network to make sure there is no conflicting IP Addresses - if there is then it would be a case of changing an IP Address on one of the devices to stop it conflicting
- Implementing storm control (Cisco devices) and configure ARP Rate limiting - this will limit the rate that ARP packets that are sent over the network while implementing storm control will look at the broadcast, unicast and multicast packets being sent to a port - if it reaches over a certain threshold then the Strom Control will take action depending on the parameters set.
- If hardware is old then it might be worth considering newer hardware as they will be able to cope with higher amounts of traffic being sent to a port such as broadcast packets lowering the expectation of an ARP Broadcast Storm being able to cause Denial of Service (DoS).

## Monitor Mode - 802.11 Analysis
<p>Context: This file contains traffic information from a wireless network.</p>
<p></p>File Size: 1.97 MB</p>
<p>File Name: Monitormode_packet_capture.pcap</p>
<p>File Type: .pcap</p>
<p>Time: 2020-07-16 04:28:16 </p>

As part of my projects - one of the things I wanted to take a closer look at was the 802.11 protocol for wireless networks as predominantly I've been looking at Ethernet based attacks where as more people are starting to adopt wireless networks within homes and enterprise settings for devices such as IoT and Access Points for Wifi. For these reasons I think it is good to be able to look at how Wireless packets appear in Wireshark and being able to identify de-authentication and more. So lets get started.

#### Starting Analysis

To start at the pcap - I want to look at some of the information that are availbale in the packets - things such as transmission rate, channel frequency just general things to be able to find them in a packet. I can do this by opening a packet and looking at the <code>Radiotap Header</code> in the packet to find out the information. Here is what is available in this pcap:

![Channel Freq](https://github.com/user-attachments/assets/cc6fe7ee-4a32-4cf0-a325-edf353ab527d)

> Channel Frequency

![Data Rate](https://github.com/user-attachments/assets/eff24296-c17d-427b-ba9f-d27d70b35ea4)

> Data Rate

![Signal Strength](https://github.com/user-attachments/assets/b0fede84-1618-4f01-9465-547f8e7cbb0f)

> Signal Strength

Another packet header than can provide me some useful information about the network is the <code>IEEE 802.11 Wireless Management</code>. This part of the packet header contains information such as the beacon interval time, SSID and more information. Looking below can show some of this information:

![Beacon - Interval](https://github.com/user-attachments/assets/29a7c885-1fcf-4cbe-8e4e-8e3cb376ec56)

> Beacon Interval Time - time between consecutive beacon transmissions

![Beacon - Parameters](https://github.com/user-attachments/assets/3ca1b6dc-1dc0-4745-a281-2bd77b805513)

> Parameters set for the Beacon

![Beacon - SSID Info](https://github.com/user-attachments/assets/b928f58e-b9dd-4108-8d9f-4470b0fecdbb)

> SSID Information

Within a packet capture - this information can be useful to find which network a user may have connected to at a certain timeframe but also tell us detailed information about the Wifi network as it could be used in attacks such as a MiTM attack. One other header that is useful to see more information about the beacon is <code>IEEE 802.11 Beacon Frame</code> which can tell us the MAC Address of the source and destination of transmission and any WLAN flags that are included:

![Beacon - Source Address](https://github.com/user-attachments/assets/957dcf61-61b5-4165-a008-09f12643af5f)

> Beacon Source Address (MAC)

All of this information can be useful in an investigation regarding a wireless attack as it can give information on the beacon of the WIFI network - but there is also some more frames which I can take a look at which gives some more information.

#### Control, Data and Management Frames

This section looks at frames such as control and data frames that are used in the 802.11 handshake as well as management frames so lets have a look. Within Wireshark I can apply filters to look for specific frames within the pcap to limit the search:

![Control Frames](https://github.com/user-attachments/assets/dd03f028-1ad1-4998-809c-7ddbc23148dd)

> Control Frames

![Data Frames](https://github.com/user-attachments/assets/ee1dde20-3e98-425b-a45e-6319e4cf1c5d)

> Data Frames

![Management Frames](https://github.com/user-attachments/assets/eb76b4f1-11bd-4b07-aeb3-b5f64868ae5d)

> Management Frames

Within the 802.11 protocol we have the association process which looks a little something like this:

![802 11 Association](https://github.com/user-attachments/assets/9f217286-8c90-4184-bdde-0ae4d9407954)

> Source: Cisco Meraki Documentation: https://documentation.meraki.com/MR/Wi-Fi_Basics_and_Best_Practices/802.11_Association_Process_Explained

Looking at these seperate frames I can see the parameters set within each part of the assoication process which I will open up now:

![Assoication Request Frame](https://github.com/user-attachments/assets/e6c0e7fb-fa1b-44f2-be87-11f466df71a8)

> Association Request Frame

![Assoication Request Response](https://github.com/user-attachments/assets/4e8b7f3f-58e3-4db3-89e8-3d02fec0412f)

> Association Request Response Frame

![Authentication](https://github.com/user-attachments/assets/181086a5-abc4-48d4-8169-b631c8cbcee2)

> Authentication

One of the most important ones to see if the de-authentication frame as in deauth attacks - this frame can give some information away about why the deauthorisation happened which I'm going to show below:

![De-Authentication](https://github.com/user-attachments/assets/a6a773a4-bedc-4ba6-bfdd-183042237f63)

> De-Authentication

Just to have a quick look, when a client wants to join a wireless network - the process looks something like this:

![Client Authentication Process](https://github.com/user-attachments/assets/5276f751-f760-44e3-bf0f-4d4bbd4bb66c)

> Client Authentication Process

## Overall 

This project gives a basic over look at the 802.11 protocol within Wireshark and how to look at certain frames as well as information about beacon points as part of a wireless network. There are more frames etc. that can be looked at within Wireshark - and as I love a nice cheat sheet I've found one online that provides Wireshark filters to be able to use. Overall I found this project quite interesting as it visibly shows the process of authentication between a device and beacon on a network which is more than what meets the eye. Wireshark filters are below:

![Wireless Wireshark Filters](https://github.com/user-attachments/assets/cdadcefc-aa73-447a-a251-6a18943e1759)
