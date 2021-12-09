---
layout: single
title:  Playing with a Chinese PoC for a VMware CVE! (For fun and ZERO profit) 
date: 2021-12-9
classes: wide
header:
  teaser: 
tags:
  - CVE
--- 

## Introduction 
The CVE in question is CVE-2021-21972.

I archived it in the event the repository goes down.

https://web.archive.org/web/20211030123608/https://github.com/NS-Sp4ce/CVE-2021-21972

![](/assets/images/chinese-poc/cve.png)

Obviously don't run this script on your own machine without proper precautions.

I'm going to be using a Kali Linux virtual machine in VMware to sandbox harm from my actual computer.
Speaking of sandbox, I made a snapshot of my virtual machine and ran the script. 

![](/assets/images/chinese-poc/cve2.png)

On the first run it didn't execute properly so I investigated line 258 and removed the problem code. (I couldn't be bothered to fix the actual problem)

I tried understanding the script afterwards to understand what's going on under the hood. Here's an extremely surface level breakdown. 

-Initializing variables for use later, for example 🠓🠓🠓

# init vulnerable url and shell URL
VUL_URI = "/ui/vropspluginui/rest/services/uploadova"
WINDOWS_SHELL_URL = "/statsreport/shell.jsp"
LINUX_SHELL_URL = "/ui/resources/shell.jsp"

-Verify whether the url is vCenter or VCSA and what version. Check if it's vulnerable

-Upload corresponding .jsp payload (windows or linux) to vulnerable url, check for success

--------------------------------------------------------------------------

## Chinese C2 Fun Times!!!!

If the upload is successful you will be faced with text that says

 "[+] Shell exist URL: {url}, default password:rebeyond"

With no further instructions on how to manage it. Going into the closed issues on the Github page I came across 

![](/assets/images/chinese-poc/cve3.png)

 I downloaded "Behinder" from the releases section of the Github, unzipped it, and attempted to run the jar file held within.
 
 
![](/assets/images/chinese-poc/cve4.png)

Unsure of what this error message says (I can't read Chinese) I got to googling and by assumption I determined that the issue had to do with "Javafx" whatever that is. 

I downloaded the SDK and moved the "lib" folder to where the jar file was located. Ran the command again and was pleasantly surprised. 

![](/assets/images/chinese-poc/cve5.png)

Took some fiddling but I eventually bumped into how to add my shell. From there I had access to a shell to escalate privileges or drop a payload as a user.

 hypothetically

If you want a breakdown of the CVE itself from the author check out: 

https://swarm.ptsecurity.com/unauth-rce-vmware/










