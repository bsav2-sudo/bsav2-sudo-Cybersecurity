# 4. Personal Memory Forensics

![Image](https://github.com/user-attachments/assets/a6958dd8-0428-4803-9ee6-df97c131186d)

### Skills Utilised

<code>Memory Foresnics</code> <code>Redline</code> <code>FTK Imager</code> <code>Eric Zimmerman Tools</code> <code>Windows Forensics</code> <code>Registry</code> <code>Volatility</code> <code>RAM Dumps</code> 

## Overview

This project looks at doing Memory Forensics - specifically for Windows Systems utilising different tools to find out infomation about a certain machine. This is useful in a scenario where a machine has been compromised and needs to be investigated to find behaviour that led to the machine being compromised. This is a useful skill to have in the world of Cybersecurity as it will allow for reporting on incidents more clear and accurate from generating information from said compromised machines. Furthermore - being able to use tools such as Eric Zimmerman tools and Registry Editor for me allows me to understand what is happening at a machine level with Network Interactions, User Behaviour and unique characteristics - which in future can prevent attacks leading to compromised machines being detected and irradicated more efficiently.

For this project I will be using my lab machine as the "compromised" machine - this machine has not been compromised in any way however will evidence a proof of concept of being able to collect information from an infected machine. So lets get started!

## Memory Forensics - Windows

### Collect Evidence

When generating evidence as part of a Memory forensics investigation - one of the main and most important parts to collect is the memory dump from the system. A memory dump is all the information from the computers working memory (RAM) and creating a copy of this. The RAM contains information about what was happening on the system before the system was compromised or crashed which is useful for us in an investigation. There are two ways in which I like to create a Memory Dump file:

#### FTK Imager

One of my favourite tools is the FTK Imager available from Exterro - this application allows me to create an entire disk image or dump the memory from the RAM to be able to use for investigation. 

![FTK Imager](https://github.com/user-attachments/assets/5d04e186-7e01-49ac-9cbd-5efa5dd9e18b)

In this instance I am going to take a dump of the RAM using FTK Imager which is as simple as clicking the File > Capture Memory option and then selecting where I can choose where to save the dumped file. This is what the process looks like:

![Capturing RAM Data Dump](https://github.com/user-attachments/assets/f970bd9f-fc2f-449a-8c4a-e8204b76b070)

When its complete a dump file will be created and can be used for the next stages.

#### Dump It

Another tool that can be used is DumpIt. Dump It is a command line tool that does the same job as FKT Imager and dumps the RAM into a memory dump file. This is what the process looks like:

![Dumping RAM from System](https://github.com/user-attachments/assets/91af3f4c-8641-42b0-b732-ec23af30bb94)

Great now I have created a memory dump of my lab system - so now its time to move onto what can be analysed from this file.

### Analysis - Memory Dump

In this part of the project - I will be using multiple tools to collect information from the memory dump file I just created. This memory dump can contain information about what the user has been doing in the lead up to an event, so lets get started with one of my favourite tools.

#### Volatility3

Volatility3 is an open source memory foresnics framework that allows an investigator to extract and analyse the digital forensics from volatile memory (RAM). One of the most exciting things about volatility is the fact it has support for Windows, Mac and Linux memory dumps and a lot of support for an extensive range of file formats. For me when completing this project - Volatility can give me a lot of valuable information about the state of the system before point of compromise. So lets get started with Volatility3.

![Volaitility GUI Running](https://github.com/user-attachments/assets/17dd36ee-e8fb-4169-8794-d29fcf0a34de)

In the GUI version of - I can see the options that are availbale to me:

- Image File - Import memory dump file
- Platform - select which platform the dump file is from
- Command - be able to run the commands from Volatility to the dump file

I prefer to use the GUI version of Volatility3 just as it allows me to visibly see all the command options avalable to me compared to the command line version. So the first thing I'm going to do is add my memory dump to Volatility and let Volatility3 look at the dump file. From there it will let me choose the commands that I can use with Volatility.

One of my favourite things is the Volatility3 Cheat Sheet that I can find online which gives me all the available commands for Windows Volatility. 

![Volatility-3-for-Windows-Cheatsheet-scaled](https://github.com/user-attachments/assets/2942506c-b532-4d02-a0fa-4c180d7e3e40)

Some of my favourite commands are the following:

- PSTREE - Prints the process tree from the memory dump
- NETSTAT - Displays network statistics, routing tables, connections etc.
- REGISTRY HIVE - Displays keys and subkeys from memory dump
- PRIVELEGE - Privelages of each user from memory dump
- SERVICE ID'S - Displays Service ID's from memory dump
- OS VERSION - Displays Version number of machine

I've ran some of these commands on Volatility3 and got the following outputs:

Windows Info:

![WindowsInfo](https://github.com/user-attachments/assets/5093eaf6-51f2-44d8-9808-3585149adfeb)

Netstat:

![Netstat](https://github.com/user-attachments/assets/21c088fa-170c-41a4-8cff-d4fa151afab4)

Registry Hive:

![Registry Hive](https://github.com/user-attachments/assets/cf7f00ad-6ae8-403f-a41f-f5238a6aeb66)

DLL Scan:

![DLL Scan](https://github.com/user-attachments/assets/ca71e680-4038-4e56-9052-aca422473ffe)

There are a lot more commands that can be run to collect evidence but this shows a proof of concept of collecting evidence using Volatility3.

All of these outputs can be used for analysis to see for example what ports are open and communicating on the network, any suspicious executables that are running that can't be seen, any suspicious services that are running and more.

### Registry Explorer - Eric Zimmerman

One of my other favourite tools to use for Windows Forensics is Eric Zimmerman's Registry Explorer. Windows Registry is collection of databases that contain configurations for a system. The data within these configurations can be anything such as the hardware information for the system, software, users information on the system and more. From a forensics standpoint - being able to access this sort of data when conducting an investigation is really useful as it can give more clues or evidence as to the events leading up to a compromised system. 

There are 5 main root keys that are in any Windows System:

- HKEY_CURRENT_USER - User who is currently logged in
- HKEY_USERS - All Loaded User Profiles on the Machine
- HKEY_LOCAL_MACHINE - Config information specific to the computer
- HKEY_CLASSES_ROOT - Registry Hive that contains file extensions
- HKEY_CURRENT_CONFIG - Information about hardware profile that is used on computer startup

Within these root keys there are pieces of information which can be very useful to me in an investigation into a system, I'm going to explore some of them thatc contain really useful information.

Ziya DENIZ on Medium uses a really nice chart that shows the different artifacts that are available from Registry Hives which contain files that are really good for getting more info:

![Medium - Artifacts](https://github.com/user-attachments/assets/c3c17740-47c3-4d61-ab73-a7362f141a67)

Source: https://medium.com/@dziyaaa/registry-forensic-analysis-317192c9cf59

#### Artifacts within Registry Hives

Lets have a look at some of the artifacts that are available from the Windows Registry Hives

- NTUSER.DAT - <code>HKEY_CURRENT_USER</code> - Contains User Information
- AmCache - <code>C:\Windows\AppCompat\Programs\Amcache.hve</code> - Information on programs recently run on the computer
- Background Activity Monitor - <code>SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}</code> - Information on programs running in the background of the system
- ShimCache - <code>SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache</code> - Keeps a track of App Compatability on the computer
- Network List - <code>SOFTWARE</code> - contains information on Network names, connection dates and more

There is a lot of artifacts that can be recovered from the Registry Hive - I've went through the effort to do a practical of as many of them as possible - this is available upon request. This ZIP file contains screenshots of me accessing the registry hives and getting informaion from the artifacts. Get in touch to have a look and see!

**Note**

This is an ongoing project - I'm still exploring the MacOS and Linux Forensics so keep an eye on this page for more!



