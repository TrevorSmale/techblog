+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 2"
date = "2025-04-05"
description = "Second week of the PSC we get deeper into STIGs"
draft = "false"
tags = [
  "Tech", "Engineering", "Security", "ProLUG Course"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Intro 👋

This week covers more implementation of Secure Technical Implementation Guidelines and we look at LDAP (Light Directory Access Protocol) Installation and Setup.
This unit also introduces foundational knowledge on analyzing, configuring, and hardening networking components using tools and frameworks like STIGs, OpenSCAP, and DNS configurations.

---

### `Discussion Post 1`

`Preface`

There are 401 stigs for RHEL 9. If you filter in your stig viewer for sysctl there are 33 (mostly network focused), ssh - 39, and network - 58. Now there are some overlaps between those, but review them and answer these questions

`Question` 1. As systems engineers why are we focused on protecting the network portion of our
server builds?

`Answer`

- Most attacks come through the network
- Misconfigured services can expose critical ports.
- Data in transit is vulnerable without proper encryption and access control.
- External exposure often increases the attack surface for things like brute-force attempts, malware injection, or unauthorized access.

`Question` 2. Why is it important to understand all the possible ingress points to our servers that
exist?

`Answer`

- Ingress points = potential paths of attack. Unexpected ingress can be exploited.
- Zero-trust environments rely on strict control and observability of ingress.
- Compliance and auditing require accurate records of what’s accessible.

`Question` 3. Why is it so important to understand the behaviors of processes that are
connecting on those ingress points?

`Answer`

- Security posture depends on visibility
- Attackers scan for overlooked vulnerabilities
- Automation tools (e.g., Ansible, Terraform) can introduce new ingress points unknowingly during updates.
- Incident response is much faster and more effective when engineers understand the network surface.

---

### `Discussion Post 2`

Intro to the scenario[^3]

Read this: https://ciq.com/blog/demystifying-and-
troubleshooting-name-resolution-in-rocky-linux/ or similar blogs on DNS and host file
configurations.

`Question`

- What is the significance of the nsswitch.conf file?

`Answer`

The /etc/nsswitch.conf file controls the order in which name resolution methods are use

`Question`

- What are security problems associated with DNS and common exploits? (May have
to look into some more blogs or posts for this)

`Answer`

Core issues with DNS:

Traditional DNS can be spoofed due to a lack of built in verification.,
Queries and Responses are sent in plaintext making confidentiality an issue.,
No way to validate the source of the DNS data.,
Centralized, single point of failure.,

Common Exploits:

Spoofing (False record injection),
Flooding (Overwhelming the resolver),
Tunneling (Query based Exfiltration),
Hijacking (Modifying domain registration data),
Typosquatting (Registering similar domains) New phrase for me

---

### Definitions

- `sysctl` Linux interface to modify kernel parameters at runtime for system performance and security. 
- `nsswitch.conf` Configuration file controlling the order of name service lookups (e.g., DNS, files, LDAP). 
- `DNS: Domain Name System` translates human-readable domain names into IP addresses. 
- `Openscap` Open-source framework for automated vulnerability scanning, compliance checking, and security auditing.
- `CIS Benchmarks` Prescriptive security configuration guidelines provided by the Center for Internet Security. 
- `ss/netstat` Command-line tools to display network sockets, connections, and statistics on Unix-like systems. 
- `tcpdump` Command-line packet analyzer for capturing and inspecting network traffic in real-time. 
- `ngrep` Network packet analyzer like grep, allowing pattern matching on network traffic payloads.

## Lab 🧪

### IP Forwarding

`Question`
- Does this system appear to be set to forward? Why or why not?

`Answer`
- No. All relevant `net.ipv4.conf.*.forwarding` and `net.ipv4.ip_forward` values are set to `0`.

### Martians

`Question`
- What are martians and is this system allowing them?

`Answer`
- Martians are packets with invalid or bogus source/destination addresses. This system is not logging them (`log_martians = 0`), but whether they're allowed depends on other rules. Logging is disabled.


### Kernel Panic Behavior

`Question`
- How does this system handle kernel panics?

`Answer`
- `kernel.panic = 0` means the system won’t auto-reboot on panic. `panic_on_oops = 1` indicates it will panic on kernel oops errors. Other panic triggers are mostly disabled.

### FIPS Mode

`Question`
- Is FIPS mode enabled?

`Answer`
- No. `crypto.fips_enabled = 0`.

`Question`
- What should be read about to better understand FIPS?

`Answer`
- TODO

## Kernel Command Line

`Question`
- What are the active boot parameters from `/proc/cmdline`?

`Answer`
- TODO (values include initrd paths, UUIDs, FIPS status not explicitly shown).

### Security Settings & STIGs

##### V-257957 – TCP Syncookies

`Question`
- Is the system using TCP syncookies?

`Answer`
- Yes. `net.ipv4.tcp_syncookies = 1`.

`Question`
- How to make this setting persistent?

`Answer`
- Add `net.ipv4.tcp_syncookies = 1` to a file in `/etc/sysctl.d/`, then run `sysctl --system`.

##### V-257958 – ICMP Redirects

`Question`
- Is the system accepting ICMP redirect messages?

`Answer`
- No. `net.ipv4.conf.all.accept_redirects = 0`

`Question`
- How to harden this?

`Answer`
- Add `net.ipv4.conf.all.accept_redirects = 0` to `/etc/sysctl.d/`, then reload settings with `sysctl --system`.

`Question`
- Did you fully understand all parameter meanings?

`Answer`
- No. Some were clarified using ChatGPT.


### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

[^1]: Professional Linux User Group Security Engineering Unit 2 [Web Book](https://professionallinuxusersgroup.github.io/psc/u2ws.html) ProLUG, 2025.

