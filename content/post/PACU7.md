+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 7 🐧"
date = "2024-10-27"
description = "ProLUG Admin Course Unit 7"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Security
## Patching the system/ Package Management
### yum, dnf, rpm

![Packaged Cat](https://trevorsmale.github.io/techblog/images/PACU7/CatPackage.jpg)

As the course progresses, I am learning more deeply within a study group. Aside from the lab work and discussion posts. I have been putting a lot of hours satisfying curiosities regarding the linux system.
For this unit we did a deep dive into packaging, going so far as to look at the history and reasoning before decision making. We had also looked at packages are managed within tightly controlled environments.
I now feel that I have a robust understanding of the theory and practical elements of Redhat packaging and beyond.

## Discussion Post 1: 

Why software versioning is important. 🤔

1. Versioning enables you to monitor software updates systematically, making it easier to troubleshoot, roll back changes, and trace modifications for security or functionality purposes. 👍
2. One can manage dependencies confidently, avoiding conflicts between system components, libraries, and tools. crucial for stable and consistent deployments. 👍
3. We can verify package integrity, ensuring installed software hasn’t been altered or corrupted. Essential for maintaining a secure and stable system environment. 👍

## Discussion Post 2: 

### Scenario: 

You are new to a Linux team. A ticket has come in from an application team and has already been escalated to your manager. They want software installed on one of their servers but you cannot find any documentation and your security team is out to lunch and not responding. You remember from some early documentation that you read that all the software in the internal repos you currently have are approved for deployment on servers. You want to also verify by checking other servers that this software exists. This is an urgent ask and your manager is hovering.
How can you check all the repos on your system to see which are active? 🤔
How would you check another server to see if the software was installed there? 🤔
If you find the software, how might you figure out when it was installed? (Time/Date) 🤔

### Answer: 

In an urgent situation like this, I’d first check which approved software repositories are active on my system, 
then verify if the software is already installed on similar servers to ensure it’s safe to proceed. Finally, I’d review the installation history to confirm when it was 
added. Working with Red Hat packaging and package management systems has many more options than I was expecting; through labbing in the study group, 
I’ve gained a much better understanding of packages, dependencies, and package management.

Packing was a pretty deep rabbit hole for me. 

This is the process I'd follow for this case:

1. Check Active Repositories
    dnf repolist  

2. Check if the Software is Installed on Your System
    rpm -qa | grep <software_name>
    or
    dnf list installed <software_name> 

3. Check Another Server for Software Installation with SSH and Step 2 commands.

4. View Installation History (Time/Date)

    dnf history info <transaction_id>
    dnf history list <software_name> 

---

## Discussion Post 3 

(After you have completed the lab) - Looking at the concept of group install from DNF or Yum. Why do you think an administrator may never want to use that in a running system? Why might an engineer want to or not want to use that? This is a thought exercise, so it’s not a “right or wrong” answer it’s for you to think about.

### Question: 

What is the concept of software bloat, and how do you think it relates?

### Answer: 

Software bloat is when essential tools/packages are larger than they need to be, effecting performance, reliability and security. By performance I am referring to the loss of potential performance from unessecary resource use. In regards to reliability, more complex systems inherently have more potential to fail. Security means many things, so I am specifically thinking about attack surface and potential for vulnerability due to the aforementioned complexity. 

### Question: 

What is the concept of a security baseline, and how do you think it relates?

### Answer: 

A set of minimum security standards and controls that organizations implement to protect systems. 

### Question: 

How do you think something like this affects performance baselines? 

### Answer: 

By targeting specific packages, tracking changes, reducing unnessecary dependancies and bloat, we satisfy the tenants of a security baseline by establishing consistency, simplifying compliance, enhancing efficiency and reduce risk.

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis


[^1]: MITRE | ATT&CK [Site](https://attack.mitre.org/) Knowledge Base.