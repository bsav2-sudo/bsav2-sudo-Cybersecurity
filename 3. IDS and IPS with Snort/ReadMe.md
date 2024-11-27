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

