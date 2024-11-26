# 3. IDS and IPS with Snort

![png-clipart-snort-illustration-snort-logo-icons-logos-emojis-tech-companies](https://github.com/user-attachments/assets/12a2b460-4319-4bd9-9c0b-c8b7d6feca09)

### Skills Utilised

<code>Snort</code> <code>Network Analysis</code> <code>IDS and IPS</code> <code>Rule Making</code> <code>TCP and UDP</code> <code>IPv4</code> <code>Windows 11</code> <code>Open Source</code> <code>Network Intrusion Analysis</code> <code>Alerts</code> <code>Packet Logging</code> <code>Network Intrusion</code>

## Overview

This project looks at Snort, an Open Source Intrusion Detection and Prevention System used in personal and buisness cases for identifying network intrusions. This project looks at the Snort program, installing the software and configuring the snort.config file to be able to work on the network. This project also looks at using Snort to be able to make rules and alerts using Snort and applying them to a closed-lab scenario with also using Scapy to be able to test the alerts. Furthermore - I will also be looking at the rule making structure for Snort and how it can be used for multiple scenarios and use cases. Please note this project was undertaken on a closed environment to be able to test the rules using Snort.

## Snort 

### Install

So to start with I am going to install Snort onto my machine which is running Windows 11. The easiest way for me to do this is to install the binary as an exe fiel to get started which I'll do now. It should be noted on Linux distros such as Kali Linux have Snort pre-installed but it is always good to learn the way to download Snort from scrtach. It can also be installed using Git but for now I'm going to install the binary.

After installing the binary I'm just going to simply run it and let the installer run - installing Snort onto my C Drive. Okay, now that is done I'm going to go to the file location of Snort which looks a little something like this:

![Install Directory](https://github.com/user-attachments/assets/9a1a0f4a-970e-4bf8-a715-a6ee8ec98a86)

It is worth knowing what is in each Folder so lets go through them:

- bin - Contains the Snort applcation and DLL's
- doc - Contains Read Me files
- etc - Contains the configuration files for Snort
- lib - Contains Snort Dynamic Engine and Dynamic Rules
- log - Default for Logs
- preproc_rules - Preproctered Rules
- rules - Pre-made rules from the Snort community
- so_rules - Extra rules made from the Snort community

The one that is most useful to know is <code>bin</code> and <code>etc</code> as these contain the main files for making Snort work - the actual application and the Configuration file. Before going to the <code>.config</code> file I'm first going to check my Snort is working okay.

![Running Snort - CMD](https://github.com/user-attachments/assets/dcbb7865-076c-4fd8-be9c-8502241960cb)

Awesome the Snort application is running as expected - furthermore I can check to see what version number I am on to make sure I am up to date on the latest version:

![Snort Version Check](https://github.com/user-attachments/assets/561684d6-588a-4bcc-b684-d768d11b226a)

Cool, now its time to look at the <code>Snort.config</code> file.

### Configuration File
