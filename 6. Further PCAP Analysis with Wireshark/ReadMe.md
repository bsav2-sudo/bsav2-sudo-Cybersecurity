# 6. Further PCAP Analysis with Wireshark

![wiresharkimage](https://github.com/user-attachments/assets/3b9b807c-2c52-4498-9e8e-b389d03e88d3)

### Skills Utilised

<code>Wireshark</code> <code>IPv4</code> <code>TCP</code> <code>UDP</code> <code>Traffic Analysis</code> <code>Traffic Filtering</code> <code>Problem Solving</code> <code>Packet Headers</code>  <code>Network Scanning Techniques</code> <code>ARP Storm</code> <code>802.11 Protocol</code> <code>Fragmentation</code>

## Overview

This project looks at **3** more PCAP files that I have sourced from the internet to further explore Network Traffic analysis on different attacks. This project looks at ARP Storms, IPv4 Fragmentation and also analysis of 802.11 protocol to see how it works within Wireshark. The PCAP files have been sourced from trusted websites who provide practice .pcap files to be able to practice and identify network traffic. I wanted to explore more of the practice PCAP files to be able to further my ability and being able to identify types of network attacks. I am particularly interested in the analysis of the 802.11 protocol - as for most of my learning - I have been looking at Ethernet protocols, but wireless networks are prone to attacks as well including de-authentication attacks and more. SO lets get started by looking at an ARP Storm attack.

## PCAP Analysis 

## ARP-STORM
<p>Context: This file contains traffic information from what is a suspected ARP Storm</p>
<p></p>File Size: 46.1 KB</p>
<p>File Name: arp-storm.pcap</p>
<p>File Type: .pcap</p>
<p>Time: 2004-10-05 / 15.01.05</p>

#### Start of Analysis

This pcap file looks at what is believed to be an ARP Storm attack that has taken place on a network. The description of a ARP Storm attack is as follows:

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

#### 
