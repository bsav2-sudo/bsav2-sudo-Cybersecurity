# 4. Phishing Analysis

![Phishing](https://github.com/user-attachments/assets/20d8ae96-9db9-42c7-b87c-09b3049d29b5)

### Skills Utilised

<code>Phishing Analysis</code> <code>Email Analysis</code> <code>SPF</code> <code>DKIM</code> <code>Phishing Techniques</code> <code>Social Engineering</code> <code>Authenticity</code>

## Overview

In this project - I take a look at phishing emails from a real-world environment - analysiing them and looking at what makes these emails work and how they have been able to reach the recipient's emails without any warnings or indicators. There are a large range of repositories and databases which store emails which people have found through honeypots and more - making it a great resource to be able to grab some and analyse the elements of the emails. Phishing is the most common technique for threat actors and hackers to steal credentials and more - why you may ask - quite simply humans are the weakest part of any security - as someone who has been working in the IT field the contrast in users technical knowledge is wider than I ever expected - meaning when it comes to phishing emails - the most vulnerable users are those who have little technical knowledge and most likely to open a suspicious link. This is why a project such as this one is very useful to take into the cybersecurity field - as it is one of the most common occurences.

So with out further ado lets get started.

## Gathering Samples

For this project - I am going to be using this GitHub Repository from rf-peixoto. Then repo is full of over 1000+ emails that have been collected from a honeypot and contains a variety of different type of phishing emails from banks to music subscriptions. I've linked the repo below in case you want to have a look and try it for yourself!

[Link to rf-peixoto Repository](https://github.com/rf-peixoto/phishing_pot)

## Sample715

Lets start with the email labelled Sample715 - looking at the email - it looks to be an email indicating that we have a Bitcoin account that has been making money in the background and we have 24 hours to withdraw the funds. 

<img width="1440" alt="Message Script - including PDF" src="https://github.com/user-attachments/assets/a15059fe-5e9f-4e6c-848a-945e1bb8956a" />

Straight away this is a red flag - unless you happen to own a bitcoin account - even then a plaintext looking email asking you to withdraw money in the next 24 hours is very suspicious.

<img width="1440" alt="Subject Phrasing" src="https://github.com/user-attachments/assets/0c09fdf7-12b7-444e-af87-74b8af0e117f" />

The subject line of the email as well "+24,311$ burn out in 24 hours" doesn't really make any sense at all - an indicator that this email is suspicious. But this is coming from a person who has seen phishing emails float into their inbox all the time - to the average user this may not be as noticable - the message could seem like it is genuine as who is going to remember if they actually opened a bitcoin account one year ago?

<img width="1440" alt="PDF Attachment" src="https://github.com/user-attachments/assets/5307db6e-03fe-47ed-a7b8-8b323f3924d9" />

Next thing that is suspicious - the PDF attachment which supposedly has instructions on how to withdraw the money from the bitcoin account. Why would you put the instructions in a PDF - why would you not list them as part of the email message - why does it have to be a seperate file attached to the email. Signs of suspicion and I would be very wary of opening the PDF file straight away - I'd want to analyse it first with something like peepdf or pdfif and use a tool such as exiftool just to get some basic information about the file. 

But yet again as I say this - would your average user think to do this - very unlikely - if they haven't ignored the email at this point then they are most likely going to engage with the email and open this PDF file. But lets go a bit more in depth - how did this email not get picked up by some sort of filtering?

<img width="1440" alt="GMAIL SPF Pass" src="https://github.com/user-attachments/assets/ad25e066-0514-4c3b-ab72-154f0e183aea" />

The first sign looking at the email elements is the SPF check that occured during the transmission of the email - as you can see from the screenshot above - the email passed the SPF check as from the Outlook Protection, gmail.com is a permitted domain and so Outlook allowed the email to pass through the filtering. This is what most likely happens when phishing emails go through a SPF pass - as the check is looking for authorized domains - sending the email from a highly trusted domain such as gmail.com allows the email to pass through the checks. Below is what exactly the SPF check looks like:

![60f727264cceba66c4e18b97_SPF_record_image-01-1024x686](https://github.com/user-attachments/assets/afb20973-b1c2-4d21-aa41-462fa5a34823)

Very simply - a DNS lookup is executed to see if the domain that the email came from is authorized - if it is allowed, it is passed into transmission, but can be categorised against reputation data to go into different folders such as spam or go into quarantine.

<img width="1440" alt="Sender IP - SMTP Auth" src="https://github.com/user-attachments/assets/f74517b6-3acf-4686-abc9-6911dbfaf10b" />

The next sign looking in the message elements is the next authentication process which is DKIM (DomainKeys Identified Mail). DKIM is the process of identifying the senders domain and ensuring the email hasn't been tampered with when being sent. Looking at the results from our email - we can see that we have a sender IP of 209.85.208.67 and also gave a dkim=pass which tells us the signature was verified. 

Looking at the IP address - we get a location of what looks like a Gmail mail server in Finland...

<img width="362" alt="IP Address" src="https://github.com/user-attachments/assets/b0bd7b83-2fa6-4146-a415-05a2de582f50" />

Confirms that the email came from a gmail domain and also may have came from Finland but doesn't tell us any more about the user who sent it. Looking further down the screenshot before on the DKIM - we can actually see the details of the Google-DKIM-Signature. 

These two checks (SPF and DKIM) show how the email was able to be sent to us without being stopped on the way of transmission. This is why when a phishing email does arrive at a users email - the ability for the user to be able to look at the email and identify it's phsihing is so important - and this can be done through the correct user training and phishing simulations on what to look for in an email - which may be obvious to us as people with technical knowledge but to the average user may not be that noticable - and also being able to report these emails to IT Security so that they can implement the correct procudures such as SPF/DKIM rules or firewall rules etc.
