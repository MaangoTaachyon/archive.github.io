---
layout: single
title: Leaked Babuk Ransomware Builder Fun!! 
date: 2021-12-9
classes: wide
header:
  teaser: /assets/images/ransom-note.jpg
tags:
  - Ransomware
  - Malware analysis
--- 

Realized I had a file named "babuk_builder.zip" sitting on my computer that I haven't messed with. 
Today's the day! This isn't the cutting edge of ransomware leaks but it is interesting none the less.
![](/assets/images/babukPost/ransom-built.png)

## Objectives
In this post I will go over:

    The process of generating ransomware with their builder
    Features of the generated malware
    Uploading samples to virustotal and any.run  

-----------------------------------------------------------------------------------------------------------------------

**DISCLAIMER** 

I am not an experienced reverse engineer or malware analyst. If you are looking for high quality, polished, tutorialesque content this is not the article for you. I am not responsible for any damage you cause to your own systems or others. At the end I will include high quality resources.

**END DISCLAIMER**

 -----------------------------------------------------------------------------------------------------------------------

 
## Setup
 I started out by downloading our sample from: https://vxug.fakedoma.in/tmp

Which is my favorite malware repository and archive among other things, to a sand boxed Windows 7 installation with tools installed. 

Air gapping the VM by removing networking functionality, drag and drop/copy paste, and shared folder access to name a few measures taken. 

If you would like a more polished experience check out: https://github.com/mandiant/flare-vm  "a fully customizable, Windows-based security distribution for malware analysis, incident response, penetration testing, etc." to use the authors own words. 


## GENERATING RANSOMWARE!!

Taking a quick glance at the source code using visual studio code (what the group clearly used to write this) 

![](/assets/images/babukPost/ransom2.png)

We can get an idea of the syntax of this builder.exe that continually draws my eye. 

![](/assets/images/babukPost/ransom3.png)

Here we see that it's as simple as executing that previously mentioned exe file and the name of the folder you want to output to. 

(Upon deeper inspection there is functionality to "...pass as a second argument an actual elliptic curve encryption key, instead of letting the builder generate it for us, allowing the ransomware operator to use the same decryption executable for different builds. Furthermore, it has been observed that, if no encryption key is specified as an argument, the key would be generated randomly."
as written in: https://lab52.io/blog/quick-review-of-babuk-ransomware-builder/

## FEATURES OF THE GENERATED MALWARE

![](/assets/images/babukPost/ransom4.png)

The files follow a very simple naming convention. The prefix "d" is used to indicated that the file is used to decrypt the specified architecture (the second "syllable") 

In the following files the prefix e indicates the file is used to encrypt instead. 

The "CURVE25519" files are the aforementioned randomly generated eliptic curve encryption keys.

Today I am going to run the e_win executable and check out the results. The exsi, linux, arm and x86 files could very have profound differences. (I doubt it) 

![](/assets/images/babukPost/ransom-note.jpg)

## ANY.RUN analysis and IOCs (Indicators of compromise)
In the note dropped by an unedited note file build of babuk ransomware there is a link to this onion address:  
tsu2dpiiv4zjzfyq73eibemit2qyrimbbb6lhpm6n5ihgallom5lhdyd.onion (click at your own risk)

![](/assets/images/babukPost/ransom5.png)

## Sources:

https://blog.talosintelligence.com/2021/11/babuk-exploits-exchange.html

https://lab52.io/blog/quick-review-of-babuk-ransomware-builder/

https://blog.malwarebytes.com/reports/2021/06/babuk-ransomware-builder-leaked-following-muddled-retirement/


