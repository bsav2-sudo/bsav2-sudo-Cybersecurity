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

Now for this analysis I would normally want to use the Zui interface - but after following Zeek's official YouTube channel - I decided to use [Try Zeek](https://try.zeek.org/#/?example=hello) instead - it is a simple website that lets me use Zeek online - all I have to do is upload to pcap file and let it analyze the file. And then after doing that here is the results:

![Adding PCAP File](https://github.com/user-attachments/assets/23c29a2f-2ec7-4542-8282-ea1fa70c48ed)

![Results ](https://github.com/user-attachments/assets/5310f131-1feb-445b-98e5-c5ffa1d3d457)

As you can see - I get all of the Zeek logs it can find in the PCAP, there is a really useful cheat sheet that I was able to find on all the Zeek logs and what all the parameters means which I have linked here [Cheat Sheets](https://github.com/corelight/zeek-cheatsheets). These logs include HTTP Requests, Files, Made Connections and more logs. This makes it way simpler than looking through a whole PCAP file just to find a certain detail - I can just look at the Logs and see what has been gathered to get further into my investogation.

Okay so now I have a basic view of Zeek, let me see what I need to find out from the pcap file:

![Malware Net Overview](https://github.com/user-attachments/assets/400cdbbd-300b-4ba7-9e0c-7deb4f955e7d)

Okay so this is the info I need to find:

- IP, Host Name and User Account of Infected Client
- Other User Account and Host Name is HTTP POST Traffic
- Infected Users Email Password
- SHA256 Hashes for the two Windows PE's

### First Task

So the first task wants me to get the IP, Host Name and User Account of the infected client, lets get started. Looking at the HTTP Logs from Zeek - I can see that I get the URI information from the PCAP which includes some Host Names and User Names - lets have a look:

![User Accounts](https://github.com/user-attachments/assets/59568d5a-d6c5-4f0e-9262-7087ef22c1c5)

There looks to be two of them - and from the info I've been given I already know the bottom one is the Domain Controller which only leaves the top one as out infected client:

![Infected PC](https://github.com/user-attachments/assets/102ffedb-cac7-4903-baee-bd53bd0cc73e)

I'm pretty satisfied this is our infected client so I'll make a note on the details

<p>IP Address: <code>10.5.28.229</code></p>
<p>User Account: Yas33</p>
<p>Host Name: CAT-BOMB-W7-PC</p>

Cool lets move onto the second task.

### Second Task

One of the cool things I can do within TryZeek is export any of the logs into Wireshark to be able to look at the packets more in detail - so that's what I will do with the HTTP logs from Zeek. Looking into the information that is sent on HTTP I can see another user:

![User Accounts - Wireshark](https://github.com/user-attachments/assets/2140dd25-9d32-468e-8725-df783d80ae99)

Amazing - I can see the User Accounts which has made me actually wonder about my first answer - I think that could be philip.gent so I'll change that and also add the other two answers I've found from this:

Infected Client

<p>IP: <code>10.5.28.229</code></p>
<p>User Account: phillip.ghent</p>
<p>Host Name: CAT-BOMB-W7-PC</p>

Other Client

<p>User Account: timothy.sizemore</p>
<p>Host Name: CAT-BOMB-W10-PC</p>

### Third Task

The third task wants me to find the infected users email password so this must have been in the pcap somewhere - my first guess is going to be somewhere in the HTTP Traffic as from the second task there is a lot of text/plain traffic which I can have a look at.

Within Wireshark - I can follow the HTTP Traffic using the Follow > HTTP Stream and see what information it is - and looking at HTTP with the Stream ID of 9, there is a piece of information from POP3 that contains our infected users name and a password next to it:

![Outlook Password](https://github.com/user-attachments/assets/37a7e5e7-54e6-40d8-90f4-d5dc27a05ee5)

Password: gh3ntf@st

Grand, three down one to go!

### Fourth Task

For the final task - I need to find the two Windows Portable Executables in this pcap and get the SHA256 Hashes from them. So the first thing I'm going to do is head back to TryZeek and look at the PE Logs:

![PE Logs](https://github.com/user-attachments/assets/37af21c5-e007-4698-a75c-a94994992ad1)

Great I've found both of them and I know that they are two .png files from looking at them individually - lets get it exported as an HTTP Object - to do this I'm going to go back to Wireshark and choose the Export > HTTP Object option. From there I can filter by content type to find the two .png files:

![Export HTTP Objects](https://github.com/user-attachments/assets/ec74ca13-eb6d-4835-aeeb-3cd219347c0b)

And once I've done that they'll be exported into the destination I chose and I can run them through PowerShell to get the SHA256 Hashes for each one which is as follows:

<p>4e76d73f3b303e481036ada80c2eeba8db2f306cbc9323748560843c80b2fed1 - cursor.png</p>
<p>934c84524389ecfb3b1dfcb28f9697a2b52ea0ebcaa510469f02d9086bcc79a - imgpaper.png</p>

Great - thats all the tasks complete and looking at the answers included on the website - I got them all correct! This just proves the ease of using Zeek to complete these tasks - if I were to use the pcap just purely in Wireshark it would take me longer to find what I was looking for!


