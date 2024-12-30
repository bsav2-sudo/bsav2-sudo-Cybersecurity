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

Now its time to look at the configuration file for Snort which is stored in <code>etc</code>. The configuration file handles things such as global variables, whether certain modules should be enabled or disabled, performance settings, event logging and more. In a way I like to think of it as the settings for the appliction to be enable/disable features and enter values. So let me open Notepad++ and open the config file:

![Configuration File - Notepad](https://github.com/user-attachments/assets/709c34ed-777f-4954-a304-9a608ba6ee88)

The configuration file contains different steps that should be looked at which are the following:

- Step 1 - Set the Network Variables
- Step 2 - Configure the Decoder
- Step 3 - Configure the Base Detection Engine
- Step 4 - Configure Dynamic Load Libraries
- Step 5 - Configure Pre-Processors
- Step 6 - Confgure Output Plugins
- Step 7 - Customize Your Rule Set
- Step 8 - Customize your preprocessor and decoder alerts
- Step 9 - Customize your Shared Object Snort Rules

All of these steps from the config file also have ReadMe files associated with them to be able to see what can and can't be done within each section.

#### Step 1 - Set the Network Variables

As you can see from the first part of the configuration file - the first variables that need to be set is the network variables which include servers to include, ports to include and the IP of home and external addresses. So to start the first thing I'm going to do is set the <code>ipvar HOME_NET</code> to my IP Address for my Wireless NIC, so let me put that in now:

![Home_Net](https://github.com/user-attachments/assets/5a55b1b5-82e4-42bc-918c-0fea9d0c383b)

Great now the IP Address I want to protect is entered into the config file - I don't have any servers such as DNS or SMTP on my network so I'm going to leave these blank - but obviously if there was servers you would want to add these to the config file in the same way. 

One thing that should be noted is to disable a module or variable, you can add '#' to the front of the argument to make it a commment which then won't be used in the config file - removing it will enable it as shown below:

>> '# config ignore_ports: tcp 21 6667:6671 1356' - Disabled
>> ' config ignore_ports: tcp 21 6667:6671 1356' - Enabled

Looking through the Network Section there is nothing more I want to add to my configuration so I'll move onto the next step.

#### Step 2 - Configure the Decoder

This section focuses on being able to configure the decoder with variables such as enabling/disabling alerts for <code>TCP</code> <code>IP</code> and <code>UDP</code> as well as being able to configure the decoder for Snort. There is nothing that I need to edit within this section as all of the alerts set are exactly what I want - however there is one argument I want to add.

Within this section I can set the <code>config logdir</code> to the Directory where I would like the .log files to be saved to - in this case I want to set a new default directory so I will add the following to the config file:

![Config LogDir](https://github.com/user-attachments/assets/6b27e6ed-d180-429e-9af5-10c35c722ce3)

As you can see I have added the following argument to set a new Log Directory:

<code>config logdir: C:\Users\Ben\Documents\SnortLogs</code>

Great now to move to Step 3.

#### Step 3 - Configure the base detection engine

In this step - parameters and variables can be set for the detection engine such as being able to change event queues, latency configurations and more. In this section I am happy with all the default values that have been set but if I need to chnage these in the future then I know I can go to this section and chnage them.

#### Step 4 - Configure Dynamic Loaded Libraries

Next is to have a look at the DLL's (Dynamic Loaded Libraries) for Snort and make sure that paths to the pre-processor and engine are all set. In my instillation of Snort there was an issue with the libraries and engine not being set correctly so I need to set these up properly which I have done in this screenshot:

![Dynamic Lib](https://github.com/user-attachments/assets/47a7ec6d-e137-4e57-81be-7cbeb757bbbc)

As you can see there is now a path set for Snort to find the Dynmaic Engine and Dynamic PreProcessor which is in the lib directory of Snort.

##### Step 5 - Configure Pre-Processors

Next on the configuration file is to configure the pre-processors including the protocol settings such as ftp server defualts etc. which I have listed below - this basically allows you to change parameters to make sure the pre-processor is looking for certain values in headers:

![FTP Normalization and Detection](https://github.com/user-attachments/assets/c5268a11-25b9-4c7b-be91-bf9e3d01d3bc)

>> FTP Normalization and Detection in Snort Configuration

There is nothing I want to change within this part but I know where it is for future reference.

##### Step 6 - Configure Output Plugins 

In this section, I can chnage the parameters for outputs such as pcaps and log files for what to be included - for now I'm going to leave these as the defaults but good to know where they are within the configuration file.

#### Step 7 - Customise Rule Set

This section is for setting which rules I want Snort to include when it is active which are kept as .rules files. These include files such as expolit-kit.rules for detecting Exploit Kits and more so I can choose which ones I want to use. As with all the other ones I'm going to leave the defaults as they include all of the rules that I have acquired from the Snort Install.

#### Step 8 - Custmomeise your PreProcessor and Decoder Alerts

This section allows me to chnage the alerts for the preprocessor and decoder - for now I am happy with the default so I'm going to leave them for now.

#### Step 9 - Customise your Shared Object Snort Rules

This finally allows me to add any of the rules that I have obtained for Snort as extra rules - I have already done this and assigned the path.

#### Testing Configuration

Now that is completed - I need to make sure that all the changes I have made will still allow Snort to run properly and how I expect it to - to do this I'm going to run the configuration as a test in Snort which will identify if there us any problems with the configuration file. The command in Snort to do this is as follows:

<code>snort -c "Path to Configuration File" -T </code>
<code>-c is to use Rules File</code>
<code>-T is to Test and report on the current configuration of Snort</code>

So let me run this in my command line:

![Snort Config - Validated](https://github.com/user-attachments/assets/6893ead1-82dd-47a0-96c4-d2ba4762c2dd)

As you can see from the red highlighted box the configuration has been validated and can be used in Snort. Amazing! It should be worth noting that I will want to do this whenever I make a change to the configuration file as this will allow me to check the config is okay to use and doesn't have anything wrong with it.

And there we are that is the configuration of Snort completed - so now it is time to use it and make some rules.

### Using Snort

#### Packet Sniffing (Verbose)

Okay now is time to actually use Snort now that I have a configuration I can use. The first mode I'm going to be using is to use it in <code>Verbose</code< mode while aslo specifying my NIC to collect information from, for me this looks something like this:

<code>sudo snort -v -i eth0</code>

When running in this mode - Snort will collect all packets from the specified Ethernet Adapter (eth0) using npcap - this will run until terminated using Ctrl+C to end the session - when the recorded data will be showed at the end looking like this:

![First Run - Snort  (VD)](https://github.com/user-attachments/assets/4a2abc15-2fa1-4e0f-ba79-f733021e6ce8)

As you can see Snort will collect all the protocols on the network and give a short overview of this data.

#### Logging Mode

In this mode - Snort will capture and log network traffic while also performing content matching and will provide alerts depending on the rules I have appied to the configuration or placed as an argument. This is a handy mode as it will allow me to see the network traffic while also being able to analyse users behaviour - however it should be noted this mode is resource intensive and will produce lots of alerts. This is what the command for logging looks like:

<code>Snort -dev -l ./log -h 192.168.1.0/24</code>

#### Network Intrusion Detection Mode

This is the main mode for Snort as it will analyse all the network traffic but won't record every single packet that goes through the wire - meaning that it is less resource intensive and will only send alerts provided in the configuration file. The command for running IDS in a simple mode looks like this:

<code>Snort -c /etc/snort/snort.conf -N</code> where -N prevents Snort from creating Log Files

There are also some other commands that can change the way alerts are formated which I have started to list below:

<code>Snort -c /etc/snort/snort.conf -D</code> - Run as a Daemon in the background

<code>Snort -c /etc/snort/snort.conf -v -A none</code> - Alert Mode with **NO** Output

<code>Snort -c /etc/snort/snort.conf -v -A console</code> - Alert Mode with **Console** Output

<code>Snort -c /etc/snort/snort.conf -v -A cmg</code> - Generates CMG Style Alerts

<code>Snort -c /etc/snort/snort.conf -v -A fast</code> - Alert Mode with **Fast** File Output

<code>Snort -c /etc/snort/snort.conf -v -A full</code> - Alert Mode with **Full** File Output

<code>Snort -c /etc/snort/rules/local.rules -v -A console</code> - Alert Mode where **Configuration isn't used**

It is always important to remember to test the configuration file and make sure rules are correctly formatted before running these commands.

### Rule Making - Snort

Now is to one of the most important parts of Snort - the rule making. By making rules I can tailor make rules that fit my specific circumstances and what I want to be activity I want to be alerted to while also specifying incoming or outgoing packets I want to be dropped entirely from the network. After completing the Snort module on TryHackMe, I was intoduced to a cheat sheet which lays out what needs to be specified in a rule which I have put below:

![TryHackMe](https://github.com/user-attachments/assets/6b080a9c-9529-425c-bcc1-ccb81c992afd)

As you can see this cheat sheet perfectly outlies what should/can be included in a rule for Snort - I often look back at this sheet to make sure any rules I have made are correct and can be used before also testing my configuration in Snort before utlisiing. Below I have screenshotted some of the Rules I have made that are most likely to appear on a rule set for an organisation.

![Creating TCP-UDP Alerts](https://github.com/user-attachments/assets/71541a5e-03e0-4447-b6e2-46d4f17f2ded)

>> TCP and UDP Alerts for when IP Address has sent/recieved packets from a known abusive IP

![Drop ICMP and TCP Rules](https://github.com/user-attachments/assets/009e6c85-5399-40aa-b9ce-1b0b9546dea2)

>> TCP and ICMP drop rules for incoming packets - CVE Numbers not actual and used as an example

![Reject TCP Rule](https://github.com/user-attachments/assets/387d0e53-81fb-4c35-b56a-49276b991ac7)

>> TCP rule to reject packets from a known abusive IP Address

It is also me worth noting the different types of rules that can be used in Snort - thankfully CrowdStrike has an excellent guide that outlines the different types of rules:

![Crowdstrike](https://github.com/user-attachments/assets/a104a961-2a82-4509-bce2-25681efeec89)

>> Source: https://www.crowdstrike.com/en-us/cybersecurity-101/threat-intelligence/snort-rules/


## Conclusion

In conclusion - from using Snort and being able to test Snort on a closed network - I have learnt that Snort is an invaluable tool when it comes to IDS/IPS as it is a versatile application that allows rule making for all use cases. The ability to run a full network sniff or to run Snort in the background makes it valuable to an IT Security Operation as they can use Snort depending on their needs. Furthermore - the community written rules create a great basis to be able to start writing rules and also changing rules for specific purposes or events.

# Update - 30th December 2024

## Overview

In this update on using Snort as an IDS/IPS, I aim to try making some new rules and testing them to make sure they work correctly and are set correctly - as I have messed with rules before this should be a straight forward task however I want to test my abilities to make sure I can make some effective IDS rules to be able to be detected. So lets get started before going into 2025!

### Making the rules

So the first rule I want to make is an ICMP Rule - so if anyone tries to ping the home subnet then it can be detected and I can be notified - the first thing I'm going to do is open my <code>local.rules</code> file and open it in my editor of choice - Sublime Text. Once open I can type in the rule I want to apply which is shown below:

![Local Rules Update - ICMP](https://github.com/user-attachments/assets/3ccd7c2d-fcd8-4a90-95aa-9e2e644e5722)

What this rule basically says is: "IF any ICMP Packets get sent from anywhere to my home network on any port - tell me about it and give it this ID - revision number 1". That is the simplest way to look at it so lets get it loaded up in Snort and run it. In Snort there is a certain command that needs to be run in order to run Snort in Alert Mode which is the following:

![Running in Alert Mode](https://github.com/user-attachments/assets/1bd6dad9-c304-4f7b-ab7d-6b57f108c136)

Here is what each of the arguments means:

Q - Run in quiet mode
l - Where is the log going to be saved
i - What network adapter do you want to use
A console - Alert mode with alerts recorded in console
c - Configuration file to use

So without further ado I'm going to grab a laptop and start sending ICMP Ping packets to the machine running Snort. Ah, on my first attempt Snort wasn't picking any of them up - I feel like I might have made a mistake in the snort.conf file about the Local Rules.

Looking in the config file sure enough there it is - my rule path wasn't set for the local.rules ☹️ 

![File Path Not Set](https://github.com/user-attachments/assets/769fbe3d-0d1b-4655-a346-59722c31d87c)

Such an easy mistake to make so let me fix it - and while I'm here I'll fix any other file paths that need fixed. Cool lets try that again...

![ICMP Detected](https://github.com/user-attachments/assets/a9e90e60-628d-4f12-9ca9-df846b52e39d)

As you can see the ICMP Packets were detected by Snort and all the alerts came through on the console! Let's try another one - lets make one for an SSH attempt:

![Local Rules Update - SSH](https://github.com/user-attachments/assets/d5276f23-32f8-4e78-9e22-fb500cdf339b)

This rule basically says: "If there is any TCP Packets sent from anybody on any port to my home network on Port 22 - let me know with the message SSH Authentication Attempt with this ID and 1st Revision."

Let me grab my trusty laptop again and try and SSH onto the machine running Snort, and sure enough:

![SSH Detected](https://github.com/user-attachments/assets/ff0c810e-1c7b-4a18-9f3e-e3a6906345a8)

Snort detected it! I love how easy it is to make the Snort rules and implement them straight away - one tool when doing this I found was Snorpy which a lot of people reccomend - I'd love to try this someday to see if it can improve workflow efficiency when working with rules. I've included a screenshot of the web client:

![snorpy](https://github.com/user-attachments/assets/33318a11-a160-4f8d-ba19-06d81ba04053)

One of things I love about Snort is the Community Rules that the Snort community make and upload to the main site - this can be used by anyone for their own IDS/IPS and include 1000's of already written rules. They often include very handy info as well such as a CVE reference if any. The rules are udpated regularly and can be downloaded from here.

[Community Rules Download Link](https://www.snort.org/downloads#rules)

![Community Rules](https://github.com/user-attachments/assets/e3a1a559-6e7b-4f2e-af89-d13774a3e549)

When opening the rules file in Sublime Text we get a huge library of pre-made rules that still follow the same rule making structure:

![Community Rules - Open](https://github.com/user-attachments/assets/6e8ec32c-25e1-41c8-b2c8-1e5c3ecb41ef)

As you can also see below there are also references to CVE's where needed:

![CVE References](https://github.com/user-attachments/assets/8f61571c-d05e-4f04-a1a5-03190886a12d)

Let's say I want to implement a new rule in my Local Rules - I want to put in a rule for ICMP DoS attacks when they happen - within these community rules I'v found one - so all I'm going to do is copy and paste the rule into my Local Rules and then save the file. 

![Local Rules Update - ICMP DoS Update](https://github.com/user-attachments/assets/ddd0a8d4-e8a0-455a-bc1e-24f138fa78ed)

And it is that easy - I'm going to be looking at Snort more in the new year to see how I can generate rules such as these - so goodbye 2024, hello 2025! 🥳
