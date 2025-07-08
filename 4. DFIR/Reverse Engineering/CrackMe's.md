# Reverse Engineering - Crack Me's

![Dbg Logo](https://github.com/user-attachments/assets/fb081659-97f6-479f-a5fc-52d480c8294a)

### Skills Utilised

<code>Reverse Engineering</code> <code>Debugging</code> <code>x86 Assembly</code> <code>Dbg x64/x86</code> <code>IDA Pro</code> <code>GHIDRA</code> <code>Code Flow and Execution</code> <code>Investigation</code> <code>Operating Systems</code> <code>Windows API</code> <code>Crack Me's</code>

## Overview

In this project, I begin looking at the techniques and tools to be able to start reverse engineering some Crack Me's which are available from [CrackMes.one](https://crackmes.one/search). These Crack Me's test my skills on being able to reverse engineer programs from simple to hard relying soley on using reverse engineering tools such as IDA Pro, DBG x64 and more to be able to crack the programs. This will be the start of my journey into the world of reverse engineering and malware analysis seeing as though this area of Cybersecurity is very new to me but a challange I want to face and learn more about. From my persepctive before I can just load a piece of malware into these tools and get started - I want to learn the foundations and build up an understanding of how programs work on a low level.

For me as an individual - malware analysis is a sought after skill - the cybersecurity landscape is ever changing and being able to understand how new attacks and new malware is executed and its behaviours is highly beneficial to a defensive operation and being able to understand what threat actors are looking for. Even just by completing some of these challanges online gives a better understanding of how programs work which when moving onto Malware Analysis can give a more refined perspective on what to look for and possible indicators.

Before I get started I want to clarify that I have already watched some tutorials and done some reading on Assembly, Malware Analysis and Reverse Engineering so when I come into this project I can make an efficient start and have a basic understanding. Most of the content I have followed has been from John Hammond, Nathan Braggs and more on YouTube who all deliver very high qulaity content on the subject matters and I would more than highly reccomend looking at.

So without further ado lets get started!

## Password-cmd Write-Up 

### Program Information

<code> 📘 Name: Password-cmd </code>
<code> 👨‍🔬 Author: UnderKo </code>
<code> 💻 Language: C/C++ </code>
<code> Arch x86-64 </code>
<code> Platform: Windows </code>
<code> 📗 Difficulty: 1.3 </code>

### Getting Started

First things first, I want to know how this program behaves without looking into it - what does the program do when you launch it and what does it ask for. When launching the program we just get a blank command prompt but after entering random characters we get a message that says "ohh noooo" - okay so not as easy as we think.

<img width="868" alt="1  Program Running" src="https://github.com/user-attachments/assets/7d59662b-178a-450b-aad0-2200e9391479" />

> Executable being run with a test argument 

<p>So it looks like we need to enter a password which should give us a response that says we've cracked the password - only we don't know what that message is yet and we only know what the incorrect answer message is - however this should be enough to go off when we step into the program. My weapon of choice today is going to be DBG x64 - as a program I love it for being able to see what is being loaded into registers as well as finding strings and following everything in the memory dump - this isn't to say IDA and Ghidra don't do those things - I just like the layour and GUI DBG x64 offers 😄 so lets get it open and find our entry point in the program.</p>

### DBG x64 Operations

<img width="1439" alt="2  Loaded and Entry Point" src="https://github.com/user-attachments/assets/42bb235e-c756-41bb-877e-a557e9a3cda1" />

> Executable loaded into DBG x64 and Entry Point highlighted with red border

Cool - we can now see the program loaded into DBG x64 and as soon as we run the program we get our starting entry point for the program - awesome! When stepping into the program for the first instructions - I expect to see some environments and threads etc. being setup and I'd be right in thinking this as some of the Windows API gets called such as GetCurrentProcessID, QueryPerformanceCounter and more with different arguments being given to these API calls - what I love about this process is I can go to my search engine and search for the API call e.g. GetCurrentProcessID followed by msdn and the first result will always be the API function documentation from Microsoft, luckily Microsoft keeps these up to date and have legacy fucntions too meaning the information is all in one place! 

Anyways I am getting ahead of myself here - setting up everything for the program is great and all but that is not what we are here for - we want to see what argument we have to give the program to give some sort of success message. This is where being able to search for strings is super useful - if I **Right Click** in dbg x64 and go to <code>Search for -> All Modules -> String References</code> I get the References window which will look for strings within the programs - now obviously even though this is a small program there are a lot of strings so we need something that we know gets printed on the command prompt.

Oh I remember - that sarcastic message that says "ohh noo" - let's have a search for that in References...

<img width="1439" alt="2b  String Search" src="https://github.com/user-attachments/assets/67432e7e-32e2-46d4-9a2c-2687cbbe0cf3" />

> Search for "ohh noo" in References inside of dbg x64

Hey what do you know that's what we were looking for! And if i double click it to go to the instructions...

<img width="1439" alt="3  Main Function-ohh no comment" src="https://github.com/user-attachments/assets/8d445025-13c2-4851-855b-42bb1bd3b091" />

> Main function which contains the "ohh noo" string

That's what we want to see. I can see from this that there is two strings one which is our "ohh noo" and another one which is "!crackme!" - If I was a betting man I would be thinking that the string which is "!crackme!" is going to be the one that outputs if we are successful with the password. Looking at the two instructions before the string I think this is a safe bet.

<code> cmp dword ptr ss:[rsp+20], A </code> - The **cmp** first is going to compare to values together 

<code> jne crackme.password.7FF7546E1BC0 </code> - The **jne** is then going to jump if the outcome of that comparison does not equal each other

Okay I think I want to go with this compare function - beacuse if the comparison of the values don't equal each other - then the program jumps to the "ohh noo" string - but if they do equal each other it will bypass the jne instruction and go to the "!crackme!" string instead.

When analysing the compare instruction - there is a hexidecimal value at the end which is 1 out of the 2 compared values - when turning this into decimal we get this...

<img width="1439" alt="4  Compare " src="https://github.com/user-attachments/assets/53dad5e3-7891-49cb-b269-52d850255ccb" />

> Compare instruction with the hexidecimal value into a decimal value in the left pane

Okay now we are getting somewhere - so if both the values are equal to each other we will have the password - and if one of the values is 10, the other value must have to be 10...

Well okay then - let's try it in the program

<img width="869" alt="7  Result" src="https://github.com/user-attachments/assets/d7702cea-9bd3-4995-b590-9ff92a0c78f9" />

> Value 10 being run in the program - Password is correct

There we go mystery solved! Our password is 10. This was a nice easy one but a really good one to get my feet into reverse engineering - what a nice challange - but it's only getting to get harder from here!
