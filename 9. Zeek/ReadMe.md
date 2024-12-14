# 9. Zeek

![image (1)](https://github.com/user-attachments/assets/641ec553-f9a4-4005-996d-bb906920666b)

### Skills Utilised

<code>Zeek</code> <code>Network Analysis</code> <code>Network Foresnics</code> <code>Wireshark</code> <code> Logs and Query Data </code> <code>Layer 7 Protocols</code> <code> UDP and TCP</code>

## Overview

This project is a practical look into using the open source network monitoring tool <code>Zeek</code>. This project will take a practice pcap file from Malware Traffic Analysis and use zeek to try and find the information required from the practice pcap. So without further ado lets get started.

## Zeek

### About Zeek

Zeek is an open source networkin security tool from The Zeek Project. Zeek helps organizations understand how their networks are being used, supporting security, performance, audit, and capacity goals. With its powerful, network-optimized programming language, vibrant open-source community, and global adoption, Zeek offers the insights needed to tackle the toughest network challenges across enterprise, cloud, and industrial computing environments. For a large period of time I thought that Wireshark was the best software to analyze network traffic until I read an article on Zeek and how much more Zeek is optimised for security analysis on networks which made me want to explore it more.

### Starting Zeek

Now as my lab laptop is a Windows platform, it isn't possibe for me to use the Linux Binaries availbale from Zeek's site - however this is one alternative application I can use to still be able to use Zeek - Zui. Zui was formally Brim Security uses the Zeek logs from a pcap file to be able to carry out analysis on network captures. So I'm going to get that installed and run it on my laptop:

![Screenshot 2024-12-14 193305](https://github.com/user-attachments/assets/77ce21cc-d5b6-41d5-b3ec-1d8a96044a0d)

Cool - now before I can have a look on Malware Traffic Analysis for a PCAP I want to investigate - I need to download the Brimcap library to be able to do queries within Zui. This comes in the form of a JSON Library and is available from Brim's Git page - so let me get that downloaded and then imported into the Zui interface:

![Queries Import](https://github.com/user-attachments/assets/ad818d1e-622b-4cc6-9f65-c80f90be109f)

As you can see - this imports already created queries from Brim such as Top Domains, HTTP Requests and more. Makes it much more easier to look through the PCAP file and find the information more easily. Cool now that I'm setup with Zeek in the Zui interface I'm going to pick my PCAP that I want to investigate within this. The one I will be choosing is the following:

![PCAP Online](https://github.com/user-attachments/assets/005735b2-fc36-4493-ae42-a6072ae907ff)

Looks like an interesting investigation so let me get it downloaded and then we can start looking at it.

### 2020-05-28 - CatBomber

