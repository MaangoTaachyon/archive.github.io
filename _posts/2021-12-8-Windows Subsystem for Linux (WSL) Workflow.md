---
layout: single
title: Windows Subsystem for Linux (WSL) Workflow 
date: 2021-12-8
classes: wide
header:
  teaser: 
tags:
  - Linux
  - Workflow
--- 

 In this post I'm going to go over customizations I've made to WSL to make my life easier.

The Windows Subsystem for Linux  is as the name implies, a Linux subsystem within Windows. This allows for bash scripting, (personally) easier command line management of files, among other things. 


To install wsl you should navigate to the Windows Store and choose your distribution. 

While your in the store install Windows Terminal as well have a better client for both powershell and your new wsl. Follow the instructions to complete the base installation.  


Go ahead and upgrade to wsl2 with this guide.
https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10

![](/assets/images/wsl/wsl.png)

First things first make sure to update all your packages. I personally use Debian so I'm going to use apt as the example here. Opensuse and Alpine Linux both use separate package managers. This should apply to Ubuntu as well.

"apt update"
"apt upgrade"

If you are not the root user (which you are not by default) you will have to prepend the apt commands with "sudo" to "do" this action as a super user (root).

----------------------------------------------------------------------------------------
Here are some packages I use daily along with a small description of each. To make your life easier.
You can install these on Debian based installations (Ubuntu included) using sudo apt install <package-name>

 

tmux - tmux is a terminal multiplexer that allows for you to split the screen into separate instances so to speak. Check out the souce code here 

https://github.com/tmux/tmux

 otherwise install (on debian based OS) with apt install
  
![](/assets/images/wsl/wsl2.png)
  
   ^^^^^^^^^^^^^^^^^^^^^

Spaceship - For my terminal prompt. It's a little complex to install for laymen but I like the way it looks. Check it out: https://github.com/spaceship-prompt/spaceship-prompt

Neovim - As my go to text editor, if you like vim you'll love neovim. An "upgraded" version of vim with extended support for plugins. If you like tinkering with configs this is for you.
Read more about it here: https://neovim.io/


For most other situations if I need reccomendations on Linux software, whether for a virtual machine or wsl in this instance. I consult
https://github.com/luong-komorebi/Awesome-Linux-Software

which has an enormous list of software for most occasions.

Thanks for reading!!!!
