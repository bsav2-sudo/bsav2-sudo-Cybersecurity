# 7. SIEM with Splunk Enterprise

![splunk-logo](https://github.com/user-attachments/assets/55bd4268-e84d-4a86-b523-ca814a65c3f9)

### Skills Utilised

<code>Security Information and Event Management</code> <code>Splunk</code> <code>Network Logs</code> <code>Investigations</code> <code>Security Logs</code> <code>Splunk Agent</code> <code>Windows Event Info</code> <code>Network Analysis</code> <code>Response</code> <code>Problem Solving</code> <code>Security Alerts</code>

## Overview

This project is a practical look into using a Security Information and Event Management solution (SIEM). For this project I am going to be using Splunk Enterprise, a Cisco company who offer a free trial to their SIEM solution - I will be setting up my lab laptop as a host for Splunk as well as adding the Splunk agent to my lab laptop to be able to trigger security events to pick up in the SIEM. Being able to use an SIEM solution is very important as this is where information can be gathered as part of an investigation and look at specific pieces of data as part of a large data set.

## SIEM - Splunk

#### Setup

To start with I'm going to setup my Splunk environment by first creating an account - this will give me access to Splunk enterprise on a trial basis which will allow me to use the SIEM solution. Now that is complete I can install Splunk Enterprise to my machine - this runs as a local host on port 8000 of my loopback address. And once it is installed and I sign in I am greeted with my home page!

![Starting Page - Local Host](https://github.com/user-attachments/assets/b19c67a8-5952-4d9b-ac2d-2ec5fd4e90bc)

Now comes the interesting parts of using an SIEM like Splunk - to be able to do anything I need to gather some information from a source such as event files or to add an agent to a device to be able to forward information:

![Information - Gathering](https://github.com/user-attachments/assets/95f09bdc-3c2d-48d9-86dc-1baf42a5825e)

For this project I want to send event logs from my computer to Splunk to be able to analyse, so for this I am going to choose the monitor option as this will allow me to send data from the computer to Splunk. From below you can see that Splunk asks us to select a certain way to obtain the data - for example local event logs, configuring Splunk to listen to ceratin TCP/UDP ports and more. For this project I'm going to ask Splunk to collect from Local event logs (uses Windows event logs) and then select which items are collected - for mine I want to gather the Application, Security and System logs which I'll select now.

![Data Collected](https://github.com/user-attachments/assets/177a491f-4b0b-41ad-8168-473fe5c8a18b)

Next Splunk will ask what I want to name the device that the information is being collected from - for ease I'm going to use the computer name given already - in a large envrionent using the device name can make it easier to be able to find data about the device as when you add filters it will narrow down the search (I'll show this later on).

![Input Settings](https://github.com/user-attachments/assets/ca63c58e-6721-4962-a372-016da7ac603a)

Then it's just a case of reviewing the data collection and then adding it, and that is it done and added to our Splunk instance! Next I'll take a look at Search and Reporting in Splunk.

#### Search and Reporting

Within Splunk - there are apps that can be used as part of the platform, for the purpose of this SIEM project I'm going to be using the 
