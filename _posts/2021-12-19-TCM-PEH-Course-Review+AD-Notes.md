---
layout: single
title:  TCM PEH Course Review+AD Notes
date: 2021-12-19
classes: wide
header:
  teaser: 
tags:
  - Certification
  - Review
--- 

# TCM PEH (The Cyber Mentor Practical Ethical Hacking) Course Review!

A little while ago I was gifted the PEH course by TCM from someone in his server!
TL;DR The PEH course is a wonderful introduction to pentesting and I would reccomend it to learn the basics.

The course covers:

![](/assets/images/TCMPEH.PNG)

It is frequently on discount and is worth every penny.

While this information could be found in other places, Heath's style of teaching is comprehensive and easy to follow along with.

He walks through a couple of boxes from HacktheBox.eu which really help with getting that initial grasp on your own methodology.

The buffer overflow video made binary exploitation a bit more palatable than I was expecting.


Here are some notes that I took from the Active Directory portion of the course. Not meant to be a polished guide for use by others :)

---

Attacking AD: Initial Attack vectors
- Abuse native windows features to get access. 

- Netbios and LLMNR 
- Relay Attack 
- NTLM Relay
- MS17-010, eternalblue still found surprisingly a lot on internal test
- Kerberoasting, post comprimise
- mitm6 

Top Five ways i got domain admin on your intenrnal network before lunch 


----LLMNR Poisining  
Link local multicast name resolution
Previously NBT-NT or netbios name service
basically DNS, that's what we use when DNS fails 


This is a man in the middle attack, stealing the hash
Using Responder.py from Impacket
python Responder.py -I tun0 -rdwv
we get an NTLM hash
crack with hashcat hashcat -m hashes.txt rockyou.txt  or hashcat -m hashfile wordlist

run it first thing in the morning or back from lunch because you need a lot of traffic to intercept
When we respond to the service it responds back with a username and a NTLM hash


----SMB Relay 

instead of cracking hashes that we get with responder we can instead relay those 
hashes to specific machines and potentially gain access.


cannot relay credential to the same machine
smb signing disabled
replayed user credentials must be on that machine
 
open Responder.conf file and turn off SMB and HTTP
run responder python Responder.py -I tun0 -rdwv
setup your relay ntlmrelayx.py -tf targets.txt -smb2support then wait for an event
profit 
nc ip 11000 afterwards for an smb shell
Dumping out the SAM files, they're like shadow in linux. Usernames and hashes for local users on the computer.

SMB signing disabled by default on workstation accounts but by default enabled and required on domain controllers
Discovering hosts with SMB signing disabled. You can run nessus and it will tell you. 
NMAP built in script and github SMB signing check script
NMAP - nmap --script=smb2-security-mode.nse -p445 IP/24
can attack even if it says message signing enable but not required

----IPv6 Attacks
another form of relaying, a lot more reliable. Utilizing ipv6
spoof dns for ipv6 addresses because usually no one is doing dns for ipv4
you don't have to be admin


Learned about these a little bit previously from a presentation by QCS (Queen City Skiddies) member lpha3ch0.
https://qcskiddies.com/?page_id=79

mitm6 -d domain
setup a relay attack ntlmrelayx.py -6 -t ldaps:\\domaincontrollerip -wh wpadfile -l dir
can use SMB, LDAP, IMAP and MSSQL
-6 is for ipv6 and -t is for target
loot is in html files open with firefox

---

Thanks for reading!
