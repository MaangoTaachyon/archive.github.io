---
layout: single
title:  TCM PNPT Certification Review
date: 2022-9-19
classes: wide
header:
  teaser: 
tags:
  - Certification
---
TL;DR/Summary: 

The Practical Network Penetration Tester certification exam from TCM Security is a simulated 5-day penetration test with the intent of proving a baseline level of offensive security prowess for the student. The exam environment is varied, primarily consisting of Windows machines but also contains Linux hosts. 2 additional days were given for reporting on remediation of vulnerabilities found and compliance steps to be taken. The goal of the hands on 5-day exam portion is to assess the entire environment for any and all vulnerabilities, but to pass, at a minimum you must gain administrative access to the domain controller. To be successful in the exam I would recommend studying the following topics as they were essential to passing.

```
- OSINT (Open-source intelligence collection and analysis)
- Active Directory Fundamentals
- Active Directory Attacks (LLMNR Poisoning, NTLM Relay Attacks, Kerberoasting, IPv6 attacks)
- Antivirus Evasion
- Password Spraying, Credential Stuffing, Password Cracking
- Enumeration and Exploitation of Web Applications
- Lateral and Vertical pivoting through networks
```
# The Longer Version

My background:

I came into this exam as someone who has been studying penetration testing and offensive security for several years as a hobbyist and recently as a professional. This certification reinforced a lot of what has been harder to practice and model in my own home lab. Coming from capture the flag competitions and research on independent targets where there’s zero-time limit, having a rigid and structured format for testing really reinforced my methodology and helped to improve my time management. Before scheduling the exam, I completed the “Practical Ethical Hacker”, “Practical Linux Privilege Escalation”, and “Practical Windows Privilege Escalation courses offered by TCM security. While I don’t think it was necessary for this certification, I felt both the privilege escalation courses were valuable.

The exam experience:

As a 5-day exam there was a clear sense of urgency to complete the exam, but I didn’t feel the need tocancel social gatherings or make exceptional changes to my schedule which was nice. I worked for a couple hours each day in bursts of an hour and taking short breaks. During the exam period you are given two environment resets if you manage to break something you weren’t meant to, or something deployed incorrectly. When I wanted a reset on my environment, I sent a request to the support email and got a ticket immediately. My request was taken care of within half an hour which I think is exceptional considering the number of emails they must get. The cyber range itself felt polished and the user machines themselves were populated with realistic enough files and programs. This emphasis on practicality and realism makes the PNPT unique in its approach.

Remediation and Debrief:

As I mentioned before the exam is mimicking a real, practical penetration test on a company, so a large portion of the experience is focused on remediation and reporting. Following the 5-day testing period you are given an additional 2 days to put together a report to present to the client. On a real engagement it’s standard practice to collect screenshots, logs, and vulnerabilities found during testing to later format. This advice worked well for me on the exam and made reporting and later presenting less intimidating than it otherwise would have been. On the day of the debrief I was greeted with Heath himself who was there to listen to the path I took to exploit the domain controller and what kind of fixes should be made to it. I chose to put together a PowerPoint to better organize my thoughts and have my notes in order.

Would I recommend it:

I would recommend this exam and the accompanying course (PEH) to anyone looking for an intermediate certification in the offensive security space. In my opinion it would be ideal for individuals already working information technology like developers, systems administrators, and dev ops professionals. The prior experience in tinkering and outside the box thinking is a valuable skill assessing what others might have already attempted to secure. For people not previously working in those fields but have tech experience and are motivated to learn more about information security it is still achievable but expect be familiar with the fundamentals before you get to the fun stuff.


