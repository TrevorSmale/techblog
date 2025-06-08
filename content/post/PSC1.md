+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 1 🔒"
date = "2025-03-23"
description = "First week of the PSC we hit the ground running with getting set up with STIG tools"
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

I've just started a new **Security Engineering** course created by Scott Champine through ProLUG. As a graduate of his **Linux Administration** course and an active contributor to the **Professional Linux User Group**, I felt compelled to make time for this new course—I've learned a great deal from his teachings in the past.

---

## Worksheet

### `Discussion Post 1`

`Question`

- What is Security?

`Answer`

In regards to Cyber Security, Integrating protective measures throughout the system lifecycle to ensure the system maintains its mission/operational effectiveness, even in the presence of adversarial threats. 

`Question`

- Describe the CIA Triad.

`Answer`

The **CIA Triad** is a core model in systems security engineering.

1. **Confidentiality** – Preventing unauthorized disclosure of system data or resources, often enforced through access control, encryption, and information flow policies.

2. **Integrity** – Ensuring that system data and operations are not altered in an unauthorized or undetected way, including protection against both accidental and intentional modification.

3. **Availability** – Ensuring reliable access to system services and resources when required, even under attack or component failure.

`Question`

- What is the relationship between Authority, Will, and Force as they relate to security?

`Answer`

In systems security engineering:

- **Authority** is derived from policy and design requirements—what the system *must* enforce according to mission objectives, laws, or standards.
- **Will** represents the commitment of system stakeholders to implement and maintain security measures.
- **Force** is the application of engineered mechanisms—technical, administrative, or procedural—that ensure security objectives are realized in practice.

`Question`

- What are the types of controls and how do they relate to the above question?

`Answer`

- In systems security engineering, **controls** are safeguards built into the system to achieve security objectives. They align with **Authority, Will, and Force** as follows:

1. **Administrative Controls** – Derived from organizational policy (Authority) and guide design, personnel roles, and security governance.
2. **Technical Controls** – Engineered into the system as part of architecture and software/hardware features (Force), e.g., encryption, access enforcement, secure boot.
3. **Operational Controls** – Rely on human procedures and configurations to maintain secure operations (Will and Force), such as patch management and monitoring.
4. **Physical Controls** – Provide physical protection to system components (Force), e.g., secure facilities or tamper-evident hardware.

---

### `Discussion Post 2`

Intro to the scenario[^3]

Find a STIG or compliance requirement that you do not agree is necessary for a server or service build.

`Question`

What is the STIG or compliance requirement trying to do?

`Answer`
The compliance requirement encourages users to set up automated CVE patch updates from trusted providers within a 24-hour timeframe.

`Question`

What category and type of control is it?

`Answer`  
This STIG is an administrative control. Since it is not built into the system by default, it must be applied and managed manually.

`Question`

Defend why you think it is not necessary. (What type of defenses do you think you could present?

`Answer`
Initially, I found it difficult to identify a STIG procedural that I disagreed with. However, after extensive review, I selected this one. I believe automated patching is not ideal, especially for production systems. Patches can introduce unexpected behaviors in dependent systems. Additionally, relying on automation can foster complacency or a lack of awareness over time.

### STIG

```stig

Apache Server 2.4 UNIX Server Security Technical Implementation Guide :: Version 3, Release: 2 Benchmark Date: 30 Jan 2025
Vul ID: V-214270           Rule ID: SV-214270r961683_rule           STIG ID: AS24-U1-000930       
Severity: CAT II           Classification: Unclass           Legacy IDs: V-92749; SV-102837

Group Title: SRG-APP-000456-WSR-000187

Rule Title: The Apache web server must install security-relevant software updates within the configured time period directed by an authoritative source (e.g., IAVM, CTOs, DTMs, and STIGs).

Discussion: Security flaws with software applications are discovered daily. Vendors are constantly updating and patching their products to address newly discovered security vulnerabilities. Organizations (including any contractor to the organization) are required to promptly install security-relevant software updates (e.g., patches, service packs, and hot fixes). Flaws discovered during security assessments, continuous monitoring, incident response activities, or information system error handling must also be addressed expeditiously.

The Apache web server will be configured to check for and install security-relevant software updates from an authoritative source within an identified time period from the availability of the update. By default, this time period will be every 24 hours.

Check Text: Determine the most recent patch level of the Apache Web Server 2.4 software, as posted on the Apache HTTP Server Project website. If the Apache installation is a proprietary installation supporting an application and is supported by a vendor, determine the most recent patch level of the vendor’s installation.

In a command line, type "httpd -v".

If the version is more than one version behind the most recent patch level, this is a finding.

```


---

### Definitions

- `CIA Triad` Core principles of security — Confidentiality, Integrity, and Availability — guiding protection of information systems.  
- `Regulatory Compliance` Adhering to laws and regulations governing data privacy, security, and operational practices.  
- `HIPAA` U.S. healthcare regulation enforcing privacy and security of patient health information. 
- `Industry Standards` Best practices and technical guidelines agreed upon within specific industries to ensure consistency and security.  
- `PCI/DSS` Payment Card Industry Data Security Standard for protecting cardholder data during processing, storage, and transmission. 
- `Security Frameworks` Structured guidelines for managing cybersecurity risks and controls within organizations. 
- `CIS` Center for Internet Security provides globally recognized benchmarks and security controls.
- `STIG` Security Technical Implementation Guide — DoD configuration standards for securing IT systems and software. 

## Lab 🧪🥼

# MariaDB STIG Remediation Lab – Q&A Format

## Signing into Remote Host

`Question`
- How do you connect to the remote host?

`Answer`
- `ssh mchammer@prolug.asuscomm.com` with password `SecLab12#$5`


## Initial Setup

`Question`
- What tools should be installed?

`Answer`
- `tmux` and `vim` via `dnf install tmux vim -y`

`Question`
- Why use tmux?

`Answer`
- To persist sessions (e.g., for using `nohup`).


## Installing and Verifying MariaDB

`Question`
- How is MariaDB installed and started?

`Answer`
- `dnf install mariadb-server`, then start and verify with:
  ```bash
  systemctl start mariadb
  systemctl status mariadb
  ss -ntulp | grep 3306
  ```

`Question`
- How do you enter the MariaDB shell?

`Answer`
- Run `mariadb`.


## V-253666: Listing Users & Max Connections

`Question`
- How do you view users and max connections?

`Answer`
- Run:
  ```sql
  SELECT DISTINCT user FROM mysql.user;
  SELECT user, max_user_connections FROM mysql.user;
  ```

## V-253677: Shutdown on Audit Failure

`Question`
- What is the issue?

`Answer`
- MariaDB must shut down or alert on audit failures like lack of space.

`Question`
- What is the fix?

`Answer`
- Configure alerting when log space is low using the OS or DB logging tools.

`Question`
- What type of control is this?

`Answer`
- Technical, detective control.

`Question`
- Is it set on your system?

`Answer`
- Not by default. Logging rollover may need to be configured manually.

## V-253678: FIFO Audit Logging

`Question`
- What is the issue?

`Answer`
- MariaDB must overwrite oldest audit logs when storage is full (FIFO).

`Question`
- What is the fix?

`Answer`
- Use syslog or configure log rotation/purging.
  Example configuration:
  ```ini
  [mariadb]
  server_audit_output_type = 'syslog'
  ```

`Question`
- What type of control is this?

`Answer`
- Technical, detective control.

`Question`
- Is it set on your system?

`Answer`
- TODO

---

## V-253754: Audit on Security Object Change

`Question`
- What is the issue?

`Answer`
- Changes to roles, privileges, and security objects must be audited.

`Question`
- What is the fix?

`Answer`
- Configure MariaDB Audit Plugin with the following:
  ```sql
  DELETE FROM mysql.server_audit_filters WHERE filtername = 'default';
  INSERT INTO mysql.server_audit_filters (filtername, rule)
  VALUES ('default',
     JSON_COMPACT(
        '{
           "connect_event": ["CONNECT", "DISCONNECT"],
           "query_event": ["ALL"]
        }'
     ));
  ```

`Question`
- What type of control is this?

`Answer`
- Technical, detective control.

`Question`
- Is it set on your system?

`Answer`
- Not by default, but was configured on the remote server.

---

[^1]: Professional Linux User Group Security Engineering Unit 1 [Web Book](https://professionallinuxusersgroup.github.io/psc/u1ws.html) Source, 2025.


