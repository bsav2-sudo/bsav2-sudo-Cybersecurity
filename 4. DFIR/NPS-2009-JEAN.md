
# 4. NPS-2009-JEAN

### Skills Utilised

<code>Digital Forensics</code> <code>Autopsy</code> <code>Artefacts</code> <code>Investigation Skills</code> <code>Forensics</code> <code>Automated Analysis</code> <code>Social Engineering</code>

## Overview

This is a practical look at conducting Digital Forensics using a "real-life" image. The task comes from the website Digital Corpora who do a lot of practical digital forensic examples online for people to self study on or walkthrough on how to perform digital forensics on disk images. In this task I will be trying to uncover what happened in this scenario which is listed below from Digital Corpora's website:

#### Scenario Details

The M57-Jean scenario is a single disk image scenario involving the exfiltration of corporate documents from the laptop of a senior executive. The scenario involves a small start-up company, M57.Biz. A few weeks into inception a confidential spreadsheet that contains the names and salaries of the company’s key employees was found posted to the “comments” section of one of the firm’s competitors. The spreadsheet only existed on one of M57’s officers, Jean.

Jean says that she has no idea how the data left her laptop and that she must have been hacked.

You have been given a disk image of Jean’s laptop. Your job is to figure out how the data was stolen, or if Jean isn’t as innocent as she claims.

## Let's get started

For these tasks I often like to use Autopsy (Sleuth Kit), I've been working with this tool for a little bit now in TryHackMe labs and I really want to use it on a practical scenario to tets if my skills have paid off. So downloading the image file provided by Digital Corpra and getting it open in Autopsy - the first thing I want to check is the supposed "spreadsheet" that Jean has created just to get an idea of the spreadsheet.

Navagting to Jean's home directory and to her Desktop - I found the spreadsheet

![1b  XLSX - Data](https://github.com/user-attachments/assets/5c5293a8-5f10-4b55-b4dd-95aed42aa24d)

As we can see the data in this xlsx file matches what Jean said she had - this is data on those within the company, salaries, positions and also SSN's!!!!! Not a good thing to have leaked. Looking at the metadata for the file it doesn't look like there has been any tampering with information so it should be safe to move on.

![1  XLSX FIle INfo](https://github.com/user-attachments/assets/25ba38e9-40c7-408b-88d0-15b32cca9f86)

LOoking at the PowerPoint that is provided with this task - there are some interviews that were conducted as part of the investigation - let's have a look at who said what:

![Screenshot from 2025-07-06 20-01-20](https://github.com/user-attachments/assets/a436c2cc-c09a-4c6f-b95e-ea20f7ba2842)

Okay so something has happened over email - so lets look for Jean's locally saved emails - this is in form of a .pst file which I can find within Jean's Application Data - and this is it here:

![2  Outlook PST](https://github.com/user-attachments/assets/51f2a0f2-f10f-4ab3-8c0f-94ece7dd9068)

Opening the .pst file and lookimg through the emails - there is an email where Jean is sending the xlsx document to "Alison" - this is the email in question:

![3  Suspicious Email](https://github.com/user-attachments/assets/f71e5bb3-f51e-40fa-82f8-7e35d9c91d0e)

Something doesn't seem right about this - possibly sent to someone trying to impersonate Alison - if we check the OriginalMessageID I can see where the original message actually came from and when we look at it...

![4  Email Information](https://github.com/user-attachments/assets/09f2ea0f-92be-4c65-baa9-1363f981f6ac)

Well that isn't Alisons email - looks like it went to an external email address - this is the point in which the data could have been obtained and spread - looks like a spearphishing attack on Jean - it doesn't look like anyone else was involved.
