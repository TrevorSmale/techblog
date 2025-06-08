+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 4 🔒"
date = "2025-04-21"
description = "Bastions and Jailing Users"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "CapstoneProject"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

## Intro 👋

Bastions and airgaps are strategies for controlling how systems connect—or don't connect—to the outside world.[^1]

---

## Worksheet

### `Discussion Post 1`

https://aws.amazon.com/search/?searchQuery=air+gapped#facet_type=blogs&page=1

https://aws.amazon.com/blogs/security/tag/bastion-host/

- Or find some on your own about air-gapped systems.

`Question`

- What seems to be the theme of air-gapped systems?

`Answer`

Air gapped systems are highly controlled and isolated systems. The degree of isolation directly correlates to the level of operational burden as modern productive systems are typically highly connected to either LANs and/or WANs.

- Blocking/Limiting/Bottlenecking Network Traffic
- Limiting Services to Bare Essentials
- Mitigating Data Egress
- Quardening off un-expected behavior
- Logging use events

`Question`

- What seems to be their purpose?

`Answer`

- To limit attack surface, mitigate malicious access and/or data infiltration/exfiltraion

`Question`

- If you use google, or an AI, what are some of the common themes that come up when asked about air-gapped or bastion systems?

`Answer`

- **Common Themes in Air-Gapped Systems**
  - Data Transfer Procedures
  - Patch Management & Updates
  - Logging and Auditing
  - Threat Models
  - Authentication & Access
  - Compliance & Certification
  - Operational Burden

- **Common Themes in Bastion Hosts**
  - Network Segmentation
  - Hardened OS Configuration
  - Jump Host Architecture
  - Access Control & MFA
  - Monitoring and Alerting
  - Change Management

- **Shared Themes**
  - Both require strict access control
  - Emphasis on tamper resistance and detection
  - Tradeoffs between security vs. usability
  - Often part of zero-trust or defense-in-depth architectures

### `Discussion Post 2`

`Question`

Do a Google or AI search of topics around jailing a user or processes in Linux.

`Answer`

**User Jailing Techniques**

- chroot 
- Namespaces  
- Control groups (cgroups)  
- Seccomp  
- AppArmor / SELinux  

**Container and Jail Environments**

- LXC  
- Docker / Podman 
- Firejail  
- Bubblewrap (bwrap) Flatpak unpriveledged namespaces

**Use Cases**

- Jailed SSH users: Using chroot in sshd_config to restrict access.
- systemd-nspawn: Lightweight containers for sandboxed environments.
- Flatpak / Snap: Sandboxed app delivery systems for desktop applications.

**Related Tools & Commands**

- chroot, unshare, setfacl, auditd
- firejail, bwrap, systemd-nspawn
- docker, podman, lxc-start


`Question`

Can you enumerate the methods of jailing users?

`Answer`

Yes there are 5 possible avenues that I know of.

`Question`

Can you think of when you’ve been jailed as a Linux user? If not, can you think of the useful ways to use a jail?

`Answer`

No I have not experienced being jailed as a user. However, if I could think of some use-cases, perhaps one would be as a honeypot for observability. Another usecase I think could work would be to trap crawlers/bots.

---

## Definitions

- `Air-gapped` Air gapped means physically isolated from unsecured networks.
- `Bastion` A bastion is a secure gateway between a trusted and untrusted network.
- `Jailed process` A jailed process is restricted to a limited portion of the filesystem.
- `Isolation` Isolation separates processes or systems to limit access and interaction.
- `Ingress` The intake of data into a system.
- `Egress` In the context of systems, having the ability. 
- `Exfiltration` When a bad actor ro program is able to extracted data from a system.
- `Cgroups` Cgroups limit and monitor resource usage of Linux processes.
- `Namespaces` isolate system resources for process groups.
- `Mount` restricts filesystem views per process group.
- `PID` isolates process ID numbers between groups.
- `IPC` isolates inter-process communication resources.
- `UTS` allows separate host and domain names.

---

## Lab 🧪🥼

process of chroot jail build

- 1. Create a chroot in /var 

```bash
mkdir /var/chroot
```

- 2. Copy in core Binaries from the system into `chroot` bin,lib64,dev,etc,home,usr/bin,lib/x86_64-linux-gnu

`Question`

What seems to be the theme of air-gapped systems?

`Answer`

- Disconnected them from regular operational activities.

`Question`

What seems to be their purpose?

`Answer`

- Reduce or eliminate the possibility of infiltration and exfiltrion.

`Question`

hat are some of the common themes that come up when asked about air-gapped or bastion systems?

`Air Gapped`

- Isolation
- Threat Mitigation
- Data Transfer Control
- Threat Mitigation
- Update Challenges
- Insider Threats
- Bridging Attacks
- Regulatory Compliance

`Bastion Hosts`

- Single Point of Entry
- Heavily Monitored
- Hardened Configuration
- Authentication Hub
- Session Recording
- Access Segregation
- Zero Trust Integration
- Threat Containment

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 4 [Web Book](https://professionallinuxusersgroup.github.io/psc/u4ws.html) ProLUG, 2025.
[^2]: Stigs that involve PAM for RHEL 9 [Website](https://docs.rockylinux.org/guides/security/pam/） Source, 2025.
[^3]: configurations of Linux via sssd [Website](https://docs.rockylinux.org/guides/security/authentication/active_directory_authentication) Source, 2025.



