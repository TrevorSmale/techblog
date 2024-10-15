+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 5 🐧"
date = "2024-10-12"
description = "ProLUG Admin Course Unit 5"
draft = "true"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Managing Users & Groups

The overarching theme of this Unit is in the title, we are looking at Managing Users & Groups. Managing users and groups in Linux within an enterprise involves creating, modifying, and organizing user accounts and permissions to enforce security and control over resources.

Organizing permissions to enforce security is more important than it has ever been, as we live in a hyper connected world with many bad actors and large amounts of sensitive data.

Linux is fundamentally well suited for Managing Users & Groups because permissions permeate every aspect of a Linux environment. Everything is a file and every file has associated permissions, Therefore we have granular control over the comings and goings of users as administrators.

## Lab Work 🧪🥼

### Unit 5 Assignment: Mapping the Lab 🗺️🤔
**Objectives:**

- Map the Internal ProLUG Network (192.168.200.0/24):
- Map the network from one of the rocky nodes.
- Using a template that you build or find from the internet 
- Provide a 1 page summary of what you find in the network.

### Approach 🤔

#### A briefing on the infra. 🔍🖥️

Het' server is unique to me. He uses an injest system that makes a jump to the actual server for security purposes. Within the main server we have a warewulf managed cluster running a series of Rocky Linux VM's. ⛰

Since we will be doing this from one of the Rocky Nodes within the system, the jump server will not be an issue I recon. 🤔

#### Mapping Strategy 📍🗺️

So I shouldn't just pop into the server and go willy nilly with scanning commands. This sever is managed by someone who understands security. So it is best to do some **Dead Reckoning** beforehand.

#### The Basic Commands ⌨️
 
Mapping the remote servers open ports with nmap.
nmap stands for Network Mapper. A quick perusal of the man page states it is an exploration tool and security / port scanner. It was designed to rapidly scan large networks. Nmap uses raw IP packets in novel ways to determine what hosts
are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics.

In order to Map the network I am going to use the ip address for the argument/target of the nmap command.

    nmap <server-ip>

Checking mounted filesystem' with df. stands for **Display Free** disk space. It is a utility that displays statistics about the amount of free disk space on the specified mounted file system or on the file system of which file is a past.

I will be using the -h flag for human readable format, which ads unit suffixes eg. Gibibyte (GiB) to the ends of values. There is more to it, but I am mentioning the bread and butter of the flag.

    df -h

### Digging Deeper ⛏️

#### Getting to know nmap 🫶

**nmap** is actually a big command, by big I mean the number of options and capabilities are vast. It is quite popular with pen testers and is packaged with Kali Linux -- a security analysis and exploitation focused distribution of Linux, so best believe it is something important. This essentially means I should take some time to get to know it more.

#### Stealth 🥷

Beware that **nmap** can and will trigger detection software like an active firewall, because **nmap** is conducting **Funny Bizniz** by way of packet **trickery** inside a network, both are technical terms. Luckily there is a **stealth** option ( -s ) that enables the mapping to take place un-detected --for the most part.

Contrary to what I assumed, the lower case **s** does not even stand for stealth, though it still helps as a mnemonic. No, it actually stands for SYN and
SYN stands for Synchronize. It is part of the TCP three-way handshake, which is a process used to establish a reliable connection between two devices on a network and has nothing to do with guys meeting at a bar.

I thought we were trying to be stealthy, not synchronized 🤔
Well is actually a form of **Funny Bizniz** wherein SYN is sent and never acknowledged, thustly not completing the handshake process and therefore hiding activity **somehow**

but **how?** Well... it reduces the chance of being logged by the target system’s monitoring tools, such as firewalls or intrusion detection systems. Now we both know. 

Keep in mind this is only one command option, just imagine how deep the rabbit hole goes.

#### Important Mapping Command List
Ninjas mark stealthier techniques. 

| **#** | **Command**                     | **Description**                                  |
|-------|----------------------------------|--------------------------------------------------|
| 1     | `nmap -sS <target>` 🥷           | TCP SYN scan (stealth mode)                      |
| 2     | `nmap -sT <target>`             | TCP connect scan (full connection)               |
| 3     | `nmap -sA <target>`             | ACK scan to detect firewalls                     |
| 4     | `nmap -sU <target>`             | UDP scan                                         |
| 5     | `nmap -sP <target>`             | Ping scan to detect live hosts                   |
| 6     | `nmap -sV <target>`             | Detect service versions on open ports           |
| 7     | `nmap -O <target>`              | OS detection                                     |
| 8     | `nmap -A <target>`              | Aggressive scan (OS, version, scripts)           |
| 9     | `nmap -Pn <target>` 🥷           | Disable ping (stealthy, avoid detection)         |
| 10    | `nmap -p- <target>`             | Scan all 65,535 TCP ports                        |
| 11    | `nmap --top-ports 100 <target>` | Scan the top 100 most common ports               |
| 12    | `nmap --script <script> <target>` | Use Nmap scripts for detailed scanning           |
| 13    | `nmap -sC <target>`             | Run default NSE scripts                          |
| 14    | `nmap -sW <target>`             | TCP window scan                                  |
| 15    | `nmap -T4 <target>`             | Faster scan using timing template T4             |
| 16    | `nmap -v <target>`              | Enable verbose output                            |
| 17    | `nmap -oN scan.txt <target>`    | Save output in normal format                     |
| 18    | `nmap -oX scan.xml <target>`    | Save output in XML format                        |
| 19    | `nmap -6 <target>`              | Scan IPv6 targets                                |
| 20    | `nmap -D RND:10 <target>` 🥷     | Decoy scan to mask source of scan                |


---

## MITRE ATT&CK - [^1] 🪚
is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. 

### Unfamiliar Terminology for me

- Lateral Movement

Lateral movement in the context of cyber exploitation refers to an attacker’s strategy of moving across a network to gain access to additional systems or sensitive data after initially infiltrating a single point. This involves leveraging compromised credentials, escalating privileges, or exploiting vulnerabilities to navigate between hosts and systems. The objective is often to broaden access within the environment while avoiding detection, eventually targeting critical infrastructure or data.

- Exfiltration 🧳🚀

refers to the unauthorized transfer of data from a target system or network to an external location controlled by an attacker. This can involve methods such as encrypted tunnels, covert channels, or compromised accounts to avoid detection. Exfiltration is typically the final step in a cyberattack, allowing attackers to steal sensitive data, intellectual property, or credentials for further malicious activities.

### Security is everybody's issue


1. What impact to the organization is data exfiltration? Even if you’re not a data owner or data custodian, why is it so important to understand the data on your systems?

Unit 5 Discussion Post 2: Find a blog or article on the web that discusses the user environment in Linux. You may want to search for .bashrc or (dot) environment files in Linux. 

2. What types of customizations might you setup for your environment? Why?

3. What problems can you anticipate around helping users with their dot files?

### Impact of Exfiltration

---

## The Linux User Environment
### Customizations to the User Environment
#### Configs
#### Why would one customize their environment

---

## Definitions/Terminology

- Footprinting 👣
- Scanning 🔍
- Enumeration 💯
- System Hacking 🪓
- Escalation of Privilege 🥉🥈🥇
- Rule of least privilege 🔐
- Covering Tracks 🧹👣
- Planting Backdoors 🪴🚪

Notes During Lecture/Class:
Links:

Terms:

Useful tools:


 






### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

[^1]: MITRE | ATT&CK [Site](https://attack.mitre.org/) Knowledge Base.