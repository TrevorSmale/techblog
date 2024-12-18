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

### What are you going to look at to explain this?

### What could be done to prevent this problem in the future?

---

## Discussion Post 2:

Your team has been giving you more and more engineering
responsibilities. You are being asked to build out the next set of servers to integrate into the
development environment. Your team is going from RHEL 8 to Rocky 9.4.

### How might you start to plan out your migration?

### What are you going to check on the existing systems to baseline your build?

### What kind of validation plan might you use for your new Rocky 9.4 systems?

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

### 1. You will scan a server for a SCC Report and get a STIG Score

### 2. You will remediate some of the items from the scan

### 3. You will rescan and verify a better score.

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

