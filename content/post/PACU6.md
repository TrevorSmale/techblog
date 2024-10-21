+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 6 🐧"
date = "2024-10-19"
description = "ProLUG Admin Course Unit 6"
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

# Security – Firewalld & UFW 🔥🧱

In this unit, we explore essential concepts in firewall management using Firewalld and UFW. A firewall acts as a security system, controlling the flow of traffic between networks by enforcing rules based on zones —logical areas with different security policies. Services are predefined sets of ports or protocols that firewalls allow or block, and zones like DMZ (Demilitarized Zone) provide added security layers by isolating public-facing systems. Stateful packet filtering tracks the state of connections, allowing more dynamic rules, while stateless packet filtering inspects individual packets without connection context. Proxies facilitate indirect network connections for security and privacy, while advanced security measures such as Web Application Firewalls (WAF) and Next-Generation Firewalls (NGFW) offer specialized protection against modern threats.

---

## Firewalld 🔥🧱

Firewalld uses zones to define the level of trust for network connections, making it easy to apply different security settings to various types of connections (like home, public, or work). It’s dynamic, meaning changes can be made without restarting the firewall, ensuring smooth operation.

---

## UFW 🔥🧱

UFW (Uncomplicated Firewall) is a user-friendly interface for managing firewall rules on Linux systems. It’s designed to simplify the process of controlling network traffic by allowing or blocking connections. UFW is commonly used on Ubuntu and provides easy commands for setting up firewall rules, making it ideal for beginners. Despite its simplicity, it remains powerful enough to handle complex configurations.

---

## WAF 🔥🧱

A WAF (Web Application Firewall) is a security system designed to protect web applications by filtering and monitoring HTTP traffic between a web application and the internet. It helps prevent common web-based attacks like SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF) by analyzing the incoming and outgoing traffic and blocking malicious requests. Unlike traditional firewalls that focus on network security, a WAF specifically targets the security of web applications and can be an important part of a layered defense strategy.

---

## NGFW 🔥🧱🧠

NGW (Next-Generation Firewall) is an advanced type of firewall that goes beyond traditional firewall features like packet filtering. It combines standard firewall capabilities with more advanced functionalities such as deep packet inspection (DPI), intrusion prevention systems (IPS), and application-level control. NGWs can inspect and control traffic at a more granular level, allowing administrators to set security rules based on specific applications, users, or behaviors.

NGWs are also capable of detecting and blocking modern threats like malware, encrypted traffic, and other sophisticated attacks. They offer real-time threat intelligence and can dynamically adjust to emerging security risks.

---

## Lab Work 🧪🥼

### Primary Commands / Tools

---



---

### Case Study #1

A ticket has come in from an application team. Some of the servers your team built for them last week have not been reporting up to enterprise monitoring and they need it to be able to troubleshoot a current issue, but they have no data. You jump on the new servers and find that your engineer built everything correctly and the agents for node_exporter, ceph_exporter and logstash exporter that your teams use. But, they also have adhered to the new company standard of firewalld must be running. No one has documented the ports that need to be open, so you’re stuck between the new standards and fixing this problem on live systems.
1. As you’re looking this up, what terms and concepts are new to you?
2. What are the ports that you need to expose? How did you find the answer?
3. What are you going to do to fix this on your firewall?

---

### Case Study #2

A manager heard you were the one that saved the new application by fixing the firewall. They get your manager to approach you with a request to review some documentation from a vendor that is pushing them hard to run a WAF in front of their web application. You are “the firewall” guy now, and they’re asking you to give them a review of the differences between the firewalls you set up (which they think should be enough to protect them) and what a WAF is doing.
1. What do you know about the differences now?
2. What are you going to do to figure out more?
Prepare a report for them comparing it to the firewall you did in the first discussion.

---

### Definitions

Firewall
Zone
Service
DMZ
Proxy
Stateful packet filtering
Stateless packet filtering
WAF
NGFW

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis


[^1]: MITRE | ATT&CK [Site](https://attack.mitre.org/) Knowledge Base.