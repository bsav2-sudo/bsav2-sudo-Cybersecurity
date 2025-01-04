# 4. Personal Memory Forensics

![Image](https://github.com/user-attachments/assets/a6958dd8-0428-4803-9ee6-df97c131186d)

### Skills Utilised

<code>Memory Foresnics</code> <code>Redline</code> <code>FTK Imager</code> <code>Eric Zimmerman Tools</code> <code>Windows Forensics</code> <code>Registry</code> <code>Volatility</code> <code>RAM Dumps</code> <code>Storage Forensics</code> <code>CLI</code> <code>Information Gathering</code>

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

There is a lot of artifacts that can be recovered from the Registry Hive - I've went through the effort to do a practical of as many of them as possible - this is available upon request. This ZIP file contains screenshots of me accessing the registry hives and getting informaion from the artifacts. Get in touch to have a look and see! I've included some of the screenshots below as an example of some of the artifacts:

![Command Line - Live System](https://github.com/user-attachments/assets/7ec653d6-2ed6-4d09-a07b-32ff3a57668c)

> Shell Bags on a Live System being exported - SBECmd Tool

![Typed URL's - IE](https://github.com/user-attachments/assets/79eef934-d229-4a46-b7a1-146f60cdfe75)

> Internet Explorer Behaviour - Registry Viewer

![USB Storage Device](https://github.com/user-attachments/assets/53304f69-f33c-4f49-87c9-cd87ac04a06c)

> USB Storage Forensics - Registry Viewer

![User Activities Database](https://github.com/user-attachments/assets/52a2e26c-70fe-4b77-8d25-871053636fef)

> User Activities Database - AppData/Local

![Apps and Services on Startup](https://github.com/user-attachments/assets/442845e9-6e68-4c4c-90ca-d2a00e268e25)

> Apps and Services on Computer Startups - Registry Viewer


**Note**

This is an ongoing project - I'm still exploring the MacOS and Linux Forensics so keep an eye on this page for more!

## Conclusion

In conclusion - from this project there is many lessons I have learnt, the biggest one being the ability to perform memory forensics on a compromised computer is a very useful skill in Cybersecurity as it can help a lot in investigations and also being able to mitigate risks in future attacks - including factors such as how the attack vector was able to access the machine, if any measures can be put in place to stop this type attack from happening again.

Another lesson I have learnt during this project is the fact that being able to gather artifacts from both live and frozen systems is a game changer in an investigation - the more artifacts that are able to be recovered - the more evidence there may be to collect from said artifacts.

And finally - the one lesson I have also learned during this project is being able to use tools such as Eric Zimmerman's tools save a lot of time in collecting artifacts as they will automatically generate a file at the end to be able to analyse.

# Mandiant Redline Forensics 

![C_AtHAgXgAAEp5t](https://github.com/user-attachments/assets/39e899cf-2bb7-49d3-bf74-56757f65bb66)

## Overview

This part of the Personal Memory Forensics chapter in my portfolio looks at using Mandiant's IOC Collector and Redline to conduct forensics on a comprimised system. Redline is a memory analysis tool that allows cybersecurity professionals to gather crucial information from a computer’s volatile memory (RAM). It assesses the computer by analyzing memory artifacts, such as running processes, network connections, loaded drivers, and open files. These artifacts help identify potential indicators of compromise (IOCs) and detect malicious activities. Using Redline and IOC Collector is a powerful way to collect information when doing an incident response. The programs are both open-source and are used widely by Cybersecurity proffesionals so having an understanding of the programs and how it works is really worth knowing. So without any further discussion lets get started.

### Install

The first thing to do is to install both Mandiant's IOC Collector and Redline. The IOC Collector is used to create IOC files (Indicators of Comprimise Files) that can be used with Mandiant Redline to conduct a IOC Collection session on a comprimised machine as part of an incident response. These are both available from the links below on Mandiant Marketplace:

[Redline](https://fireeye.market/apps/211364)

[IOC Collector](https://fireeye.market/apps/S7cWpi9W)

Now that I have these both installed the first thing to do is to boot up Redline and get started in setting up a Standard Collection.

### Running

So when I first boot up Redline I am introduced to this screen:

![RedLine - Standard](https://github.com/user-attachments/assets/192d9729-c543-4ec4-b869-3b43f4375371)

There's 3 different options to be able to choose from:

- Standard Collector
- Comprehensive Collector
- IOC Collector

For this example I'm going to use the standard collector which will configure Redline to the needs to the assesment I want to run, so I'm going to choose that option. The next thing to do is to select the target platform. I can choose either Windows, Mac OS or Linux - when we get the batch file at the end we can run that batch file from a USB to collect the evidence on different platforms (it should be noted some versions of Windows and Mac OS aren't officially supported so it is best to look at the supported list of Operating Systems), as I am doing this on my lab laptop I will be selecting Windows:

![Starting Session](https://github.com/user-attachments/assets/4ec3fa8a-dba9-4494-8bbf-7c58a6b281ea)

The next thing to do is look at the script for the collector and see what we want to collect:

![Review Script Configuration](https://github.com/user-attachments/assets/dde7b67c-f7d5-43cc-9803-bb3b02a20349)

Let's look at all the options that we can choose from:

#### Memory
![Script - Memory](https://github.com/user-attachments/assets/a021d171-b962-4633-bf14-1b65d340f1c0)

#### Disk
![Script - File](https://github.com/user-attachments/assets/4f884fa5-2e99-4790-9d2b-d639c6d2578e)

#### System
![Script - System](https://github.com/user-attachments/assets/cb688de5-cb8c-45f5-ae52-81e1f63002b9)

#### Network
![Script - Network](https://github.com/user-attachments/assets/1c2cc5dd-ebcf-482d-82fd-3505d87147a8)

#### Other
![Script - Other](https://github.com/user-attachments/assets/f49698a6-d12f-418c-92f6-dc17ee1aba88)

As you can see there is a lot of options I can choose for the Script to tell Redline what to look for on the system - some of these will depend on the type of investigation that the user of Redline wants to conduct so for now I'm going to choose only a few more options just to show the potential of Redline and what it can do.

The final thing to do is to choose a destination file for our collection - which needs to be empty as this is where all of the evidence and sessions will be saved.

![Session - Collector Location](https://github.com/user-attachments/assets/70bf6871-b124-42b1-9627-86d40056c440)

Now that is complete, I can click "Okay" and now I am presented with this message which tells me I am ready to run the batch script Redline has made.

![Collector - Instructions](https://github.com/user-attachments/assets/58e8efde-3ee3-465f-a080-9cd1c3a58271)

### Start of Analysis Session

Going to the destination file I chose I can now see the "RunRedlineAudit.bat" file which is now ready to be run as an Administrator which will start the collection.

![Run - Batch File](https://github.com/user-attachments/assets/5573fd4d-67b5-4019-b689-b1751ca76209)

The command prompt window will close once the session is complete and then I can start the analysis in Redline.

### Analysis 

From the collector finishing - I get a .mans file within the directory which can be used with Redline to look at the information that Redline has been able to find on the system.

![Analysis - Session Created](https://github.com/user-attachments/assets/b204f61e-7b69-44f1-b90f-0b3fc23ae7b8)

What is interesting to note as well is within the directory - Redline has saved the evidence as HTML files and can all be seen here below:

![Analysis - All Files](https://github.com/user-attachments/assets/8651ad59-0080-4ba3-b0f2-e01304f3dbb7)

The first thing to do is to go back into Redline and select "Analyze Data" and "From a Saved Memory File" - this is where I direct Redline to the .mans file and start the analysis on Redline which will then go through all the files the collector has found:

![Analysis - Creating Session in Redline](https://github.com/user-attachments/assets/c063ce90-2c13-4bc9-9050-8070ef72be0f)

When I first open the analysis on Redline - the first page I am introduced to is a "Start Investigation Page" which is directly on the home page - this pae makes it easier to tell Redline what type of investigation I am conducting and what information I need to find - for example do I need to find out what the user has been searching for - in which case I would tell Redline "I am Reviewing Web History Data" in which Redline will direct me to the URL History from the collector.

![Analysis - Start Investigation](https://github.com/user-attachments/assets/e0f58ed6-3b43-4d88-b0b0-e6bc0fbe6041)

It is good to know this is here, but for the time being I'm going to go through all of the data Redline provides us so lets go through them all:

#### System Information

Provides information about the Operating System, Uptime, BIOS information and much more - very useful! (Please note, my OS is currently Windows 11 but Redline records it as Windows 10 - unsure as to what may of caused this but Windows 11 is one of the unsupported Operating Systems on RedLine - no information about this from my research).

![Analysis - Sys Info](https://github.com/user-attachments/assets/981c485b-82c2-4221-ba1d-b359ad40b75a)

#### Processes

Provides information on all of the processes that are on the system:

![Analysis - Processes](https://github.com/user-attachments/assets/b9bcb059-e6be-4417-b6d6-7fcf7a337d6f)

#### Hierarchy Processes

Provides information on the processes relation to each other through a hierarchal tree:

![Analysis - Hierarchal Processes](https://github.com/user-attachments/assets/76a66b83-cf4d-4376-9871-d823a23b7b0b)

#### Registry

Shows all of the registry information from the system:

![Analysis - Registry](https://github.com/user-attachments/assets/b4b59de2-5d5c-4adc-a339-5d346c93af40)

#### Users

Shows all users on the system and also which groups they are part of:

![Analysis - Users](https://github.com/user-attachments/assets/5a416d2f-b2dc-43d0-9baf-6930f934669d)

#### DNS Entries

All of the DNS Entries that could be found on the system:

![Analysis - DNS Entries](https://github.com/user-attachments/assets/0e09eb12-9b70-461d-a9f1-979365c1c0a7)

#### Disk and Volumes

Disks and volumes that are on the system:

![Analysis - Disk and Volumes](https://github.com/user-attachments/assets/1448f8eb-f85e-429c-9a23-d8e326cee99e)

#### Windows Registry Hives

All of the Registry Hives that Redline could find on the system including SAM, Software and Security:

![Analysis - Registry Hives](https://github.com/user-attachments/assets/699d4e66-1af6-4214-ab2b-57a0a0d418f9)

#### Browser URL Records

URL Records that have been found from the system including file URL records the user has recorded:

![Analysis - URL Records](https://github.com/user-attachments/assets/745a4b31-34fc-4c34-a5d5-dcc9f229da55)

#### Timeline

Timeline of all the events on the system - great little feature if looking at a certain timeframe!

![Analysis - Timeline](https://github.com/user-attachments/assets/52758ccf-bda2-4ac5-835c-1c175cc878f8)

Now that I have seen all the information Redline can give - it is time to look at the IOC part of Redline in which Redline uses an IOC file to look on a system to try and find a match, so lets have a look!

### IOC

Mandiant has a seperate software called IOC Editor - this lets me create an IOC criteria for Redline to use to look for on a system to see if there is any match on the system - this is great if I know exactly what I am looking for or any characteristics of an IOC I want to be able to find on the system. The first thing to do is to open IOC Editor 

![Madiant IOC Editor](https://github.com/user-attachments/assets/11e825b3-bade-43fd-ad48-47dacce2fb02)

Next I can create a new IOC file which is pretty easy - just need to click File -> New IOC Indicator. Next I can fill in the information fields and then add my indictaors I want Redline to look for - this could be something such as a SHA256 Hash, an IP Address, Process ID and much much more! It is a great tool as it gives a lot of options as to what I can choose to look for in the system. Absolutely grand stuff! So I've filled out some info as a proof of concpet and now I have a IOC file ready for Redline to use - all I need to do is save it to a directory and there we go!

![Indicator Made](https://github.com/user-attachments/assets/0b2a041b-4d54-484e-b835-5814e65c25d6)

Next is to go back to Redline and choose the "IOC Search Collector" option:

![IOC Search Collector](https://github.com/user-attachments/assets/efe44463-96b2-4d63-b134-613e8e9ae4da)

From there I can select my directory where all the IOC files are saved and choose the IOC File I want to use in the collection of data:

![Search Collector - IOC Config](https://github.com/user-attachments/assets/6ac597fc-0eb8-425a-81de-95e0e799bec2)

From there I get the same options as before on a Standard Collector and then I run the batch file - from here it is pretty much the same process as I will get a .mans file I can use for analysis and see if there was any matches on the IOC.

![Search Collector - Rest of info](https://github.com/user-attachments/assets/36a7b5cc-6ea0-4e2e-ae6d-e291bc2a9e85)

Honestly I think the IOC Editor with the IOC Collector is amazing, if I have a large collection of IOC files that I have of known malware etc. then it makes it a really streamlined process to choose an IOC and then look for it on a system - the whole Redline process is really nice and really good as an investigative tool - definetly one for the future to keep using and use in real world scenarios!
