+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 13 🐧"
date = "2024-12-15"
description = "ProLUG Admin Course Unit 13"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems", "STIG", "System Hardening"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# System Hardening

Linux system hardening involves securing the system by reducing its attack surface through measures such as disabling unnecessary services, enforcing access controls, applying security patches, and using tools like OpenSCAP, STIG compliance frameworks, or the OSCAP Scanner. These tools help automate security audits, enforce compliance standards, and identify vulnerabilities to enhance system security.

---

## Discussion Post 1: 

Your security team comes to you with a discrepancy between
the production security baseline and something that is running on one of your servers in
production. There are 5 servers in a web cluster and only one of them is showing this
behavior. They want you to account for why something is different.

### How are you going to validate that the difference between the systems?

I am going to assume that I am new to the system in general and have very surface knowledge from fellow staff. I am also assuming we are working with a redhat based system.

#### Starting off simple

Maybe the problem is an obvious one, so I would just start off with a glance.

  - Quick cursory check (Kernel Version uname-a)
  - Manually Checking Logs (Journalctl, dmesg, audit.log, syslog)
  - Checking ports (Socket Statistics, ss -ntulp)
  - Listing installed packages (DNF list, RPM -qa)
  - Listing users and Logins (/etc/passwd, w, last)
  - Seeing what System D services are running (systemctl list-units - -type=service)
  - Digging for documentation and or commit history

#### Deeper ⛏️

If no low hanging fruit were there, then I would check configurations

  - Grub.conf, FirewallD/Apparmour, SELinux,

#### Sorting 🪰'ish from 🌶️
If I do that see something distinctly different, I would employ a more sophisticated approach with difference checking.
Given that everything is a structured file, I can append the output from a working system and the goose 🪿 to a new file and run diff against them.
  - Diff'ing the Logs
  - Diff'ing Socket Statistics
  - Diff'ing Installed Packages

### What are you going to look at to explain this?

I think I have answered this above.

### What could be done to prevent this problem in the future?

- Introducing or Improving the change management policy and employing version control would be my first suggestion.
- Ensuring that there is a build/test/deploy pipeline that integrates tightly with change management.
- Using IaC and Automation to ensure consistency and repeatability with tools like Ansible, Packer, Podman or Kubernetes.
- Hardening systems with either simple policies or through the guidance of STIG's.
- Introducing stronger controls over user privileges like employing RBA policies.

---

## Discussion Post 2:

Your team has been giving you more and more engineering
responsibilities. You are being asked to build out the next set of servers to integrate into the
development environment. Your team is going from RHEL 8 to Rocky 9.4.

### How might you start to plan out your migration?

#### Observe 
Firstly I would gather system information
- Benchmark/baseline performance metrics and utilization (Disk, I/O, PS, Connections etc)
- Configs (Scripts and configuration files)
- Installed Packages.
- users (Listing users and privileges)
- Policies (Firewall, SELinux)
- Purpose (Assessing the use of a particular system to see if may need changes/upgrades)

#### Capture
- I would snapshot the current system if possible
- If a complete snapshot copy is not possible, I would gather files essential to rebuilding a replica

#### Reconstruct
- Build it in a test VM emulating the current environment
- Template the VM for experimental changes (Adding additional tools or Configs)

#### Analyze / Optimize
- Gather business or operational requirements, perhaps the system needs enhancements
- Experiment with performance tuning
- Test new packages and/or configurations

#### Build
During the analysis and optimization phase, I would start a playbook with information gathered from previous phases.
I would build and run the playbook against VM templates until satisfied.

#### Deploy
Given the prior phases, my Playbook would be robust and capable of the transition.
However, I would ensure a robust backup and rollback plan in the case something fails.

### What are you going to check on the existing systems to baseline your build?

1. **Compute Usage**
2. **Memory Load**
3. **Disk Resources**
4. **Networking Metrics**

### What kind of validation plan might you use for your new Rocky 9.4 systems?

I would have a seperate playbook built that would validate performance against what I was observing during my VM experimentation.
Though the environment may differ from that of the VM, I would still be able to discern performance characteristics and notice any outlier differences.

---

## Digging Deeper

1. Run through this lab: https://killercoda.com/het-tanis/course/Linux-Labs/107-server-startup-process 👍

### How does this help you better understand the discussion 13-2 question?

Well when I am gathering a picture of my current security baseline, I can use some of these tools like dmesg and ss to see what possible attack surface I may have.

2. Run through this lab: https://killercoda.com/het-tanis/course/Linux-Labs/203-updating-golden-image 👍

### How does this help you better understand the process of hardening systems?

---

## Reflection Questions

1. What questions do you still have about this week?

2. How can you apply this now in your current role in IT? If you’re not in IT, how can you
look to put something like this into your resume or portfolio?

---

# Lab Work 🧪

### 1. You will scan a server for a SCC Report and get a STIG Score 👍

### 2. You will remediate some of the items from the scan 👍

### 3. You will rescan and verify a better score. 👍

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

