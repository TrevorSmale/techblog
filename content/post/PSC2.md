+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 2"
date = "2025-04-05"
description = "Second week of the PSC we get deeper into STIGs"
draft = "true"
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

- What is the significance of the nsswitch.conf file?

- What are security problems associated with DNS and common exploits? (May have
to look into some more blogs or posts for this)

---

### Definitions

- `sysctl`: Linux interface to modify kernel parameters at runtime for system performance and security. [https://en.wikipedia.org/wiki/Sysctl](https://en.wikipedia.org/wiki/Sysctl)
- `nsswitch.conf`: Configuration file controlling the order of name service lookups (e.g., DNS, files, LDAP). [https://en.wikipedia.org/wiki/Nsswitch.conf](https://en.wikipedia.org/wiki/Nsswitch.conf)
- `DNS: Domain Name System` — translates human-readable domain names into IP addresses. [https://en.wikipedia.org/wiki/Domain_Name_System](https://en.wikipedia.org/wiki/Domain_Name_System)
- `Openscap`: Open-source framework for automated vulnerability scanning, compliance checking, and security auditing. [https://en.wikipedia.org/wiki/OpenSCAP](https://en.wikipedia.org/wiki/OpenSCAP)
- `CIS Benchmarks`: Prescriptive security configuration guidelines provided by the Center for Internet Security. [https://en.wikipedia.org/wiki/Center_for_Internet_Security](https://en.wikipedia.org/wiki/Center_for_Internet_Security)
- `ss/netstat`: Command-line tools to display network sockets, connections, and statistics on Unix-like systems. [https://en.wikipedia.org/wiki/Netstat](https://en.wikipedia.org/wiki/Netstat)
- `tcpdump`: Command-line packet analyzer for capturing and inspecting network traffic in real-time. [https://en.wikipedia.org/wiki/Tcpdump](https://en.wikipedia.org/wiki/Tcpdump)
- `ngrep`: Network packet analyzer like grep, allowing pattern matching on network traffic payloads. [https://en.wikipedia.org/wiki/Ngrep](https://en.wikipedia.org/wiki/Ngrep)

## Lab




---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Example Title [Format](Link) Source, 2025.
[^2]: Example Title [Format](Link) Source, 2025.
[^3]: Example Title [Format](Link) Source, 2025.
[^4]: Example Title [Format](Link) Source, 2025.


