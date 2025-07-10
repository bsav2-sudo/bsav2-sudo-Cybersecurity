# 8. Investigation into Emerging Techniques in Cybersecurity

## Overview

This blog looks at recent news stories and cybersecurity blogs to look into the emerging techniques and attacks that are coming through in Cybersecurity - as part of my portfolio this is an important part of my personal development as keeping up to date with the latest trends and latest stories in cybersecurity to learn what the latest threats are and what might be considered a risk as part of an associations infastructure. There are many resources online from trusted news outlets to security professionals who blg about recent events. In my belief, as technology grows at the rate it is, the scope of cybersecurity will change with it at the same rate - as we get more devices such as IoT devices for smarthomes and new vunerabilities get found, being able to stay on top of the curve and identify new trends is important.

<br>

#### 3rd December 2024 - The Guardian

News Story: UK underestimates threat of cyber-attacks from hostile states and gangs, says security chief - by Dan Milmo

[The Guardian - Technology](https://www.theguardian.com/technology/2024/dec/03/uk-underestimates-threat-of-cyber-attacks-from-hostile-states-and-gangs-says-security-chief)

![Image](https://github.com/user-attachments/assets/ab9ae166-c953-4cad-b11e-5557326021a5)

I've chose to look at this news piece uploaded on the 3rd of December as from the headline it looks like an interesting story on the landscape of cybersecurity in the UK and where the country is currently at in terms of defence. The statements comes from the head of the GCHQ's National Cyber Security Centre (Richard Horne) - expressing his concerns at the state of cyber attacks the UK is getting from foreign states such as Russia and China. 

In this news article, Horne basically explains how attacks from these foreign states are becoming "more complex and have increased in frequency" over the last year. He also says that the UK as a whole is underestimating the scale of what is being faced. From a personal point have view, it has always been expressed how foreign states are trying to infiltrate UK systems and how the UK has responded to these suspected attacks. For example, at the current company I work for, the use of Hikvision cameras (manufactured in China) is prohibited from the main network and any sub-networks, however from visiting other locations such as venues and shopping centres, the use of these Hikvision cameras is being utilised everywhere. Looking at a security blog from CCTV Security Pro's, they even say the Hikvision IP cameras **are not** NDAA Compliant while also having known security vulnerabilities from research - yet it is still being used by medium - large buisnesses as a use case. More on the Hikvision camera blog here: https://www.cctvsecuritypros.com/blog/the-risks-of-dahua-hikvision-nonndaa-approved-security-cameras/

Anyways back to the main topic of discussion, the report then goes on to say that recent ransomware attacks from these adveseries attacked some high profile targets including a company who deal with blood tests for the NHS Trusts and GP Services. For me as someone entering the world of Cybersecurity, one of the things I have always been taught from the Live Event industry is to test, test and test your systems to make sure they are working as expected. Now obviously as these attacks change and attackers try and avoid firewalls and IDS/IPS's - being able to test rules and hardware is an important part of staying up to date with the latest attacks and threats. From my view - knowing every trick an attacker might try and pull like obfuscation and malicious fragmentation are easy to identify - but when a new threat comes in with a unique way of getting past said systems - being able to identify the technique they have employed and making it well known to others and systems make it easier to stop threats. Now I know this isn't available all the time due to external reasons - but even having the knowledge of how malware has been developed or what it is made up of can help identify flags that could be used by systems. It is why I have chose a liking to investigating malware samples in a sandbox environment - even at an entry-level being able to see what malware looks like in its most basic form and what it is actually doing in a system gives a better understanding of what an attacker is trying to get out of it.

What really suprised me however in this report was near the end where it has been highlighted in the past couple of months that North Korea workers have been disguising themselves as Freelance 3rd-Country IT Staff and working within UK Firms. A story about this broke out in November of 2024 where it was reported that large firms were employing these freelance staff. Story: https://unit42.paloaltonetworks.com/north-korean-it-workers/ . It goes back to my first training I undertook in my cybersecurity adventure - **humans are the weakest point** - from phishing campaigns to social engineering humans are more likely to click a suspicious link and grant access to an attack vector. It is why user teaching and learning of IT Security is so important in the case of a company because an attacker would rather send a mass phishing campaign with a click rate rather than try to infiltrate a system which takes more time with enumeration etc. With this part of the story for me it basically makes me wonder how these incidents do happen - are security systems and protocols too weak or are attackers getting more clever in the way they gain access. For these foreign actors - information and data is a weapon that can be used in other circumstances outside of the world of IT - which in turn has an affect on human lives.

As a whole the article gives a good indication of the state of UK Cybersecurity and why it is so important in this modern world we live in where we rely on technology in every part of our life - from payments to healthcare it is in every part of life and becoming more common to move what would usually be done manually to a form of technology, which in my opinion adds another possible vulnerability to anyone. 

Ben Savage
16:04 GMT 03-12-2024

-----------------------------------------------------------------------------------------------------------------------------

<br>

#### 4th December 2024 - The Wall Street Journal 

News Story: How Generative AI Is Reinventing Cyberattacks and Defenses (Q&A)

[Wall Street Journal](https://partners.wsj.com/okta/identity-is-security/how-generative-ai-is-reinventing-cyberattacks-and-defenses/)

![Generative-AI-Cybersecurity](https://github.com/user-attachments/assets/5fa74ed3-f31f-4d04-b0c3-927b82b459be)

One of the most important discussions happening in Cybersecurity at the minute is the growing rate of generative AI. From my own personal experience I have seen the grwoth of generative AI from watching my friends at university use it to complete assignments on time to now people using it as part of their daily life at work and at home to save time. Of course, there is always advantages and disadvantages to how generative AI can be used in our daily lives - one of the biggest topics surrounding AI within the IT community at the minute is it being implemented in IT Security. This article is a Q&A from WSJ Buisness on the use of Generative AI within both an attacking and defensive use case in cyber attacks - how can it be used as part of a defence and how can it be used as an attacking mechanism. 

One of the things that is spoken about within this article is the role generative AI has in the role of software developemnt - as a tool for software development the ability to generate code in a matter of seconds is amazing, makes the process more efficient and focus on different areas of the development. But within the article - it is discussed that security is being overlooked within the production of software as people try to get their products to market as quickly as possible. I can see this happening from a development scenario and can understand how security policies get overlooked - it highlights the importance of having frameworks in place to be able to not overlook possible security flaws. 

In the article it is also highlighted the way in which attackers are now using AI to create new threats and new ways to gain access. From looking on the outside and completing labs inside of Kali Linux the availability of scripts online the people can easily download and then watch a YouTube video on how to run the script. It doens't require a lot to be able to launch an attack now - and now with the use of Generative AI, anyone with little to no coding experience at all can create scripts to try and attack a network or person. Just as a little test I launched ChatGPT online and asked it two simple commmands:

- Write a python script to get information for **my** PC.
- Write a python script to get information for **someone else's** PC.

Only a slight difference in the wording but lets see what ChatGPT does:

![GPT - Own PC](https://github.com/user-attachments/assets/83b535c0-70a8-430a-8661-eef2aa2f22d2)

![GPT - Somone elses PC](https://github.com/user-attachments/assets/1e1ef816-4416-43a5-80aa-d967fbde856a)

It is clear that just a slight change in wording makes a difference to a Generative AI such as ChatGPT, but what is to stop someone running the script from the first question on someone else's PC? It is just a thought that even though some AI models have policies in place to stop malicious scripts being written - there'll be ways around that attackers can use to still use generative AI for their benefit. 

However what I find really interesting in this article is the discussion of how companies can use generative AI as a defense mechnaism from cyber attacks. I love the idea personally - and one of my favourite things discussed is teaching an AI model to spot deep fakes by being able to differentiate between real content and manipulated content. Overtime the more and more that AI model learns and gathers more data - the more it will be able to find malicious content. When I think about it from a defensive point of view - the use of Generative AI is a good concept and definelty one that in the article the writer suggests companies should be doing. But with this comes a warning which is "You just need to make sure you adopt the technology in the safest manner possible, which makes it more critical for businesses—both those using AI as a tool and those providing AI services—to rely on neutral and independent vendors that provide leading threat solutions versus monolithic platforms that think of security as an afterthought." 

The discussion behind Generative AI is one that is going to be talked about for a long time - in the cyber world a defensive and attacking standpoint - it will be interesting to see how the conversation develops over time with the projection of generative AI and cyber attacks in the future.

-----------------------------------------------------------------------------------------------------------------------------

<br>

### 2nd January 2025 - The Hacker News

Well first of all welcome to 2025! Let's see what the new year brings - and straight away an article from The Hacker News has peaked my interest already regarding the noticed increase in XSS (Cross-Site Scripting) Attacks.

[The Hacker News](https://thehackernews.com/2025/01/cross-domain-attacks-growing-threat-to.html)

![Crowdstrike](https://github.com/user-attachments/assets/100c73c6-c44a-4932-b3f1-252b52091279)

In the article, the writer raises the point that over the last year - and are an emerging technique used by some of the adveseries as the attack allows them to move laterally though and organisations systems and evade detection purely through using comprimised logins instead of "hacking" into the systems and endpoints. It is interesting that comprimised logins are being more regularly used for attacks rather than using a brute-force attack etc. as the public narrative on hackers is they do all of these fancy attacks by brute-forcing passwords or finding a specific exploit in a system - when actually being able to identify lets say an unused account is a much easier way to access a system and try to evade detection. 

This is further highlighted in the next paragraph of the article which discusses the current state of Identity Security:

"The rise in cross-domain and identity-based attacks exposes a critical vulnerability in organizations that treat identity security as an afterthought or compliance checkbox rather than an integral component of their security architecture. Many businesses rely on disjointed tools that address only fragments of the identity problem, resulting in visibility gaps and operational inefficiencies. This patchwork approach fails to provide a cohesive view or secure the broader identity landscape effectively."

It just highlights the need for security within IT Systems and how it should be a priority as part of a development process rather than as the article says an after thought which is where most of these vulnerabilities come from. In my experience - most end users don't understand the importance of Sign on techniques such as using Microsoft Authenticator - to most it is just an inconvinience to keep having to prove identity with the app to be able to access systems which is where I think a bit of User understanding needs to come in to play to inform the end user of the importance of having these systems.

One of the main talking points in this article is the need for a unified central way, consolidating threat detection and response across identity, endpoint and cloud within a unified platform. This removes the fragmentation when it comes to using different services for idenification and places everything in one service - rather than gathering logs and more from different services which saves time and makes it easier to respond to incidents and monitor activity. 

The article does provide a very good insight into the current state of XSS attacks how they are developing, so well worth more of a read on the article which is linked above.

<br>

#### 12th May 2025 - ITVX (M&S + Co-op Cyber Attacks)

So its been a while since I updated this blog - work has been heavy at the moment since the start of January as well as keeping up with my Cybersecurity knowledge and BAU tasks that I have going on. So this is my first actual look at the news that has been hitting the UK for the past couple of weeks - which is the cyber attack going on with retail giants M&S, Co-op and also Harrods.

[ITVX Article](https://www.itv.com/news/2025-05-12/m-and-s-and-co-op-what-we-know-weeks-after-cyber-attacks)

So what is this all about - basically there has been a cyber attack which first started with M&S - whose systems were comprimised meaning that online ordering systems, card payments and more are affected. The Co-op then said some of their systems had been taken offline as they had an attempted cyber attack on their systems, and then Harrods also came out and said they had suffered a cyber attack and were working to get their systems back online.

I really want to focus on M&S with this piece as they were the first ones to report the cyber attack and two weeks on from the initial statement - most of their systems aren't back online or in the same state as before the attack. Furthermore it is not just the systems that they have been affected by - user data and credentials have also been stolen (from both staff and customers) which just puts into perspective how much access the hackers have to their systems. 

So what do we know about the attack - well new information is coming to light about how the initial access that the hackers got - and when I first read it I thought it was a joke - but it's no joke. The hackers according to the article impersonated an employee with administrative access - saying that they needed a password reset and contacted the M&S IT Support team via phone to get it reset. First question that has got to be asked here is - who is the M&S IT Sservice Desk Team - is it outsourced or is it in hoouse? What permissions does the Service Desk team have for resetting high-priveleged or in this case just any privileged account passwords? Well we get the answer as apparently the hackers were able to get the password reset for the account.

Now in this day - one layer of security isn't enough and that would be right - as the next hurdle to overcome is 2FA or MFA - which M&S do have enabled for accounts. However - the hackers were able to get past this with a technique known as sim-swapping.

> Sim Swapping: Unauthorized SIM changes, sometimes called SIM swapping or SIM hijacking attacks, occur when a customer’s phone number is transferred to a different SIM card or eSIM profile under the control of a criminal. If the SIM swap is successful, the criminal may intercept the customer's phone calls and text messages to receive one-time security codes from social media, banks, credit card companies, cryptocurrency exchanges, and other financial institutions, allowing them to potentially access those accounts and cause financial and reputational harm to the customer. (Verizon, 2025)

Becuase the hackers were able to sim swap - they could authorise the MFA and get access to the account - and now they are into the M&S systems and into a high-privileged account.

Lets just take a second to pause here - this could all of easily been avoided if the IT Service Desk team didn't allow the hacker to reset the password - or even if they asked some security questions to confirm the identity of who was calling. Doing a password over the phone is always dangerous so what could have made them do this in the first place - is it something they have been told to do - is there a process in place? In my mind if the IT Service is outsourced from the company itself - it makes it a lot harder to manage and also to be able to know what procedures are in place. Also why would 1st Line IT have the ability to reset passwords? A lot of questions to be asked an so far not many answers from the retailer. I even remember when the news story first came out - I spoke to one of my friends who is a Cybersecurity Analyst and we both agreed there was something not right about the whole situation - the communication from the company to the media itself and the way in which it was being dealt with until the NCSC got involved.

Just goes to show - the fundamentals are **vital** in order to stop cyber attacks happening, if the basics aren't done correctly then how can it be expected that a cyber attack won't happen?

One final bit I found interesting is this part:

" It is believed that once they had enough access, they used M&S's Active Directory, a Microsoft product that connects internal networks and stores information. "

Next question is - it's been two weeks - surely they can see their logs and try and trace back the movement of this comprimised account through AD. If the hackers have been able to tamper with the logs then okay it makes it harder to find but surely there are some IOC's that are clear - especially as the hacker group used ransomware on may systems to take them offline. I may be thinking about this in the wrong way but I think it's clear that whatever security systems are in place for M&S IT need to be clearly looked at because in an ideal world - althought you can't prevent attacks like these (unless of course you don't give IT seervice desk password reset privileges) - you most definetly can make suitable arrangements to pick them up as soon as they do happen with logs, IOC's, SIEM solutions and much much more.

I'll leave at here today as of course - this is only reports of what has happened and we still wait to see what M&S say about the attack from their side.

-----------------------------------------------------------------------------------------------------------------------------

<br>

#### 10th July 2025 - BBC News (M&S + Co-op Cyber Attacks)

Okay so it has been a while (again I know) since I updated this blog but in the last few hours there has been some developments on the M&S Cyber attack - the police at the time of writing have made 4 arrests in the UK in connection with the cyber attack - with charges including  "Computer Misuse Act offences, blackmail, money laundering and participating in the activities of an organised crime group." This doesn't seem like such a big deal but this is the first time I have seen publically that someone/some people have been arrested over participating in activities of an organised crime group - the other charges make sense but this is the first time I have seen such a charge being bought up. And yet is M&S fully back online - no. Nearly 2 months on from the incident.

[BBC Article](https://www.bbc.co.uk/news/articles/cwykgrv374eo)

[BBC Episode - Inside the High-Street Cyber Attacks](https://www.bbc.co.uk/iplayer/episode/m002d2lh/inside-the-high-street-cyberattacks)

The BBC Episode that I have attached is a really good episode to watch and I would highly reccomend it - it follows the process of what happened during the hack and really shows how this "hack" has impacted a well-known buisness - with shelve shortages and more. It just goes to show how big of an impact any cyber attack has on the world as we start to become more of a cashless society and rely more and more on technology to store our data.

I'd reccomend watching the episode - it is a really good watch :)
