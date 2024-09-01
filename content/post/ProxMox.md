+++
author = "Hugo Authors"
title = "ProxMox is great 🏆"
date = "2024-08-26"
description = "ProxMox has become an invaluable tool for my learning and automation and self hosting"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "Certification", "LPIC", "Study Technique", "Self Hosting", "LXC", "Virtual Machines", "ProxMox"
]
categories = [
    "Learning", "Projects",
]
series = ["Projects"]

+++

<!--more-->

### Intro 👋

ProxMox is an free open source virtualisation host built atop debian. It can both run Virtual machines and Linux Containers with a host of features. 
I am continuously learning new systems and software. Prior to using ProxMox, I was installing and reinstalling Unix systems on an old laptop. This help me learn about installation, configuration and a whole host of other things. However this was very tedious and in efficient. The laptop was under powered, so running several virtual machines was not possible. I needed a multicore X86 machine with lots of memory in order to do this. Wanting to get more serious about learning systems engineering, I decided to build and x86 machine for the sole purpose of learning without fear of messing things up. While researching virtualization software I cam accross proxmox. 

### Installation

Installation is very straightforward for someone like me who has installed all of the major distros to bare-metal. It was a matter of downloading the latest ISO and creating a boot disk using Balena Etcher.
The installation process is very similar to the one debian has, a series of prompts, choosing a target disk, locale and you are off to the races.

### Accessing

Once Proxmox is installed it becomes a headless network machine. If you plug in a monitor, you will see a black screen indicating the host address and not much more. The way one interfaces with the system is through a browser on a computer tied to the local network. I open up a browser on my macbook and head to the address to see the sign in screen. You would have set up your username and password during the setup process, so it is just a matter of typing those in.

![ProxMox Dashboard](/images/pctower1.png)

### The Dashboard

Proxmox provides an easy to use dashboard that indicated system load and memory usage. Creating a virtual machine is as simple as clicking 'create new vm' and uploading a sufficient ISO for the purpose. The deep learning comes from automating this process and managing a number of machines. This is where Youtube tutorials come in handing. It helps to know a little bit about networking in order to set up self hosted services like Jellyfin or Database based services like NextCloud.

![ProxMox Dashboard](/images/proxdash.png)

### What I use it for

I like to create machine templates that I can utilize for automated provissioning with tools like Ansible and Terraform. This simulates setting up a large collection of machines that must be pre-configured and communicate amongst one another. Lately I have been creating templates using Cloud-Init images with the extension Qcow2 to build out templates that have randomized SSH keys and uninitiated hostnames, much like the way Azure, AWS or google cloud sets up PAAS.

### In summary

If you are serious about learning systems and practicing with many machines, I definately recommend setting up a ProxMox machine. Even an older PC will suffice for this purpose, though some limitation will apply; something there will take research.




