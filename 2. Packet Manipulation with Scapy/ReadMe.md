# 2. Packet Manipulation with Scapy

![Scapy_logo](https://github.com/user-attachments/assets/654ca147-c7e5-441f-85c3-ae6fa50da082)

### Skills Utilised

<code>Scapy</code> <code>Packet Manipulation</code> <code>IPv4</code> <code>TCP, IP and Ether</code> <code>Python</code> <code>Network Based Attacks</code> <code>Windows 11</code> <code>Wireshark</code> <code>IP Headers</code>

## Overview

This project looks at Scapy, a Python library used for packet manipulation and testing networks with specially crafted packets. In this project, I am going to be exploring Scapy including how to craft packets with TCP, IP and Ether as well as sending packets over the network (with looking at these packets in Wireshark for proof of concept). Scapy can also be used for Packet Sniffing on a network to be bale to look at packets sent over the network, but for this project I will be mainly focusing on the packet crafting section of Scapy. So lets get started.

## Scapy

### Install 

So to start with I'm going to install Scapy onto my Lab Machine, luckily Scapy comes in various install options from Githb, as a Python package and more. For me I am running Python 3.13 on my Windows laptop, meaning I can use the <code>pip</code> install manager to directly install Scapy to my laptop. The command is as follows:

![Install Scapy](https://github.com/user-attachments/assets/6bd114f9-44f2-4e86-9df7-0f902bf94ac2)

This will install scapy from the pip install manager and all of it's assets. Scapy does also require <code>Npcap</code> to be installed as this will allow Windows to capture raw network traffic - luckily for me I already have this installed as I have Wireshark on this PC. 

Now that I have everything I need to be able to run Scapy I can now run the Python script from my command line interface which looks a little something like this:

![Install](https://github.com/user-attachments/assets/d110679f-a085-4f63-9571-cfb69a196988)

Great! That is everything installed and I can now start using Scapy to craft some packets!

### Crafting a packet - Basic use

To craft a packet in Scapy is quite simple as I am effectively writing in Python meaning I can write a packet using the language with Scapy's library. When crafting a packet Scapy will require one of the following <code>IP</code> <code>Ether</code> <code>TCP</code> <code>DNS</code> or <code>ARP</code>. These will define what type of packet is going to be sent - I can also use multiple as they are on different layers of the OSI Model for example TCP, IP and Ether. This looks like the following to create an IP Packet within Scapy:

![Creating an IP Packet](https://github.com/user-attachments/assets/197a6e65-f111-4e5a-95eb-4d2258ed2d78)

This has now created a packet name "IP_Packet" with IP criteria. Excellent. But now I want to add information to this packet such as IP Addresses, IP version etc. To do this I can firstly see what parameters I have that I can use within this packet by using the following command <code>IP_Packet.show()</code> which will display all of the parameters.

![IP Packet Show](https://github.com/user-attachments/assets/1fa62e37-6279-4228-8006-2eaa6dcc3ae6)

Now I can see what parameters I can set which for IP is the following:

- Version - What version of IP is the packet for example 4 or 6
- IHL - Internet Header Length
- TOS - Type of Service
- Len - Length
- ID - IP Identification
- Flags - used for Fragmentation
- Frag - IP Fragment
- TTL - Time To Live
- Proto - Protocol used
- Chksum - is there checksum enabled or not
- Src - Source IP Address
- Dst - Destination IP Address

Now that I know all of my parameters let me go and set some, lets say I want to add the following information:

- Source IP: 192.168.1.20
- Destination IP: 192.168.1.1
- Length = 555

This is purely for demonstartion purposes so I'm going to add these now by doing the following:

e.g. Source IP Address - IP_Packet.src=192.168.1.20

Adding all of these in Scapy looks something like this:

![Example IP Packet](https://github.com/user-attachments/assets/168deae6-5790-457c-87f3-c98f5de3086c)

As you can see I have now added these parameters to the IP_Packet which is shown in the screenshot above. Now that the packet is crafted it is now time to send this onto the network. One thing I have done is make sure I am on an isolated network with no other users or endpoints meaning I can send this packet over the network and not cause any sort of DOS attack on myself. To send the packet over the network I'm going to use the following commmand in Scapy:

>> <code>send(IP_Packet)</code>
>> or to send multiple packets
>> <code>send ((IP_Packet), count *)</code>

This is what it looks like when sent in Scapy:

![Basic IP Packet - Sent](https://github.com/user-attachments/assets/24ed2126-53fb-4e9f-9597-5dacd91e29d6)

And there we are that is one crafted packet sent over the network - and if I now go into Wireshark I can see the packet has been sent and with the information (please note this screenshot is from my very first test with Scapy so may contain different info):

![Sending Packet (Wireshark Evidence)](https://github.com/user-attachments/assets/3bc6178f-eaad-4455-a7cb-76c9c1ce9d00)

There we are - that is a very basic packet sent over the network! Now it is time to add some more layers to a packet to be able to make a more complex and realistic packet.

### Typical Packet Types

Before crafting this complex packet however it will be worth having a look at some of the most common types of packets for IP, Ether, DNS, ARP and more. So lets start with ARP.

#### ARP

ARP Ping


ARP Cache Poisining 


#### ICMP 

ICMP Ping


#### IP

IPv3


Nestea Attack


Ping of Death



#### TCP 

TCP Ping


TCP Extremely Malformed



#### UDP

UDP Ping



