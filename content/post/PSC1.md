+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 1"
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

- What is Security?

**Response:**

In regards to Cyber Security, Integrating protective measures throughout the system lifecycle to ensure the system maintains its mission/operational effectiveness, even in the presence of adversarial threats. 

- Describe the CIA Triad.

**Response:**

The **CIA Triad** is a core model in systems security engineering.

1. **Confidentiality** – Preventing unauthorized disclosure of system data or resources, often enforced through access control, encryption, and information flow policies.

2. **Integrity** – Ensuring that system data and operations are not altered in an unauthorized or undetected way, including protection against both accidental and intentional modification.

3. **Availability** – Ensuring reliable access to system services and resources when required, even under attack or component failure.

- What is the relationship between Authority, Will, and Force as they relate to security?

**Response:**

In systems security engineering:

- **Authority** is derived from policy and design requirements—what the system *must* enforce according to mission objectives, laws, or standards.
- **Will** represents the commitment of system stakeholders to implement and maintain security measures.
- **Force** is the application of engineered mechanisms—technical, administrative, or procedural—that ensure security objectives are realized in practice.


- What are the types of controls and how do they relate to the above question?

**Response:**

In systems security engineering, **controls** are safeguards built into the system to achieve security objectives. They align with **Authority, Will, and Force** as follows:

1. **Administrative Controls** – Derived from organizational policy (Authority) and guide design, personnel roles, and security governance.
2. **Technical Controls** – Engineered into the system as part of architecture and software/hardware features (Force), e.g., encryption, access enforcement, secure boot.
3. **Operational Controls** – Rely on human procedures and configurations to maintain secure operations (Will and Force), such as patch management and monitoring.
4. **Physical Controls** – Provide physical protection to system components (Force), e.g., secure facilities or tamper-evident hardware.

---

### `Discussion Post 2`

Intro to the scenario[^3]

Find a STIG or compliance requirement that you do not agree is necessary for a server or service build.

1. **What is the STIG or compliance requirement trying to do?**

`Response`
The compliance requirement encourages users to set up automated CVE patch updates from trusted providers within a 24-hour timeframe.

2. **What category and type of control is it?**

`Response`  
This STIG is an administrative control. Since it is not built into the system by default, it must be applied and managed manually.

3. **Defend why you think it is not necessary. (What type of defenses do you think you could present?)**

`Response`
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

- `CIA Triad`: Core principles of security — Confidentiality, Integrity, and Availability — guiding protection of information systems.  
- `Regulatory Compliance`: Adhering to laws and regulations governing data privacy, security, and operational practices.  
- `HIPAA`: U.S. healthcare regulation enforcing privacy and security of patient health information. [https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)  
- `Industry Standards`: Best practices and technical guidelines agreed upon within specific industries to ensure consistency and security.  
- `PCI/DSS`: Payment Card Industry Data Security Standard for protecting cardholder data during processing, storage, and transmission. [https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)  
- `Security Frameworks`: Structured guidelines for managing cybersecurity risks and controls within organizations. [https://en.wikipedia.org/wiki/Information_security_management_system](https://en.wikipedia.org/wiki/Information_security_management_system)  
- `CIS`: Center for Internet Security provides globally recognized benchmarks and security controls. [https://en.wikipedia.org/wiki/Center_for_Internet_Security](https://en.wikipedia.org/wiki/Center_for_Internet_Security)  
- `STIG`: Security Technical Implementation Guide — DoD configuration standards for securing IT systems and software. [https://en.wikipedia.org/wiki/Security_Technical_Implementation_Guide](https://en.wikipedia.org/wiki/Security_Technical_Implementation_Guide)  

## Lab

## Signing into Remote Host
```bash
- sudo ssh mchammer@prolug.asuscomm.com
- SecLab12#$5
```
## Install Helpful Tooling
```bash
dnf install tmux vim -y
```
## Enter tmux for `nohup`
```bash
tmux
```
## Install MariaDB Server
```bash
dnf install mariadb-server

# Ensure that it is running

systemctl start mariadb

systemctl status mariadb

ss -ntulp | grep 3306
```
## Open MariaDB CLI
```mariadb
mariadb
```
## Remediating V-253666
### Listing Users and Max Connections:
```mariadb
SELECT DISTINCT user FROM mysql.user;
```
```mariadb
SELECT user, max_user_connections FROM mysql.user

#Exit mariadb sub shell
exit;

#Exit mariadb CLI
exit;

```
---
## Answering Questions
### V-253677 STIG
**Rule Title:**
MariaDB must by default shut down upon audit failure, to include the unavailability of space for more audit log records; or must be configurable to shut down upon audit failure.
### What is the problem?
**Discussion:**
It is critical that when MariaDB is at risk of failing to process audit logs as required, an action is taken to mitigate the failure. Audit processing failures include software/hardware errors; failures in the audit capturing mechanisms; and audit storage capacity being reached or exceeded. Responses to audit failure depend upon the nature of the failure mode.
When the need for system availability does not outweigh the need for a complete audit trail, the DBMS should shut down immediately, rolling back all in-flight transactions.
Systems where audit trail completeness is paramount will most likely be at a lower MAC level than MAC I; the final determination is the prerogative of the application owner, subject to Authorizing Official concurrence. In any case, sufficient auditing resources must be allocated to avoid a shutdown in all but the most extreme situations.
### What is the fix?
**Fix Text:**
```mariadb
Modify DBMS, OS, or third-party logging application settings to alert appropriate personnel when a specific percentage of log storage capacity is reached.
```
### What type of control is being implemented?
### Todo is it set properly on your system?
---
### V-253678 STIG
**Rule Title:**
MariaDB must be configurable to overwrite audit log records, oldest first (First-In-First-Out - FIFO), in the event of unavailability of space for more audit log records.
### What is the problem?
**Discussion:**
It is critical that when MariaDB is at risk of failing to process audit logs as required, it take action to mitigate the failure. Audit processing failures include software/hardware errors; failures in the audit capturing mechanisms; and audit storage capacity being reached or exceeded. Responses to audit failure depend upon the nature of the failure mode.
When availability is an overriding concern, approved actions in response to an audit failure are as follows:
(i) If the failure was caused by the lack of audit record storage capacity, the DBMS must continue generating audit records, if possible (automatically restarting the audit service if necessary), overwriting the oldest audit records in a first-in-first-out manner.
(ii) If audit records are sent to a centralized collection server and communication with this server is lost or the server fails, the DBMS must queue audit records locally until communication is restored or until the audit records are retrieved manually. Upon restoration of the connection to the centralized collection server, action should be taken to synchronize the local audit data with the collection server.
Systems where availability is paramount will most likely be MAC I; the final determination is the prerogative of the application owner, subject to Authorizing Official concurrence. In any case, sufficient auditing resources must be allocated to avoid audit data loss in all but the most extreme situations.
### What is the fix?
**Fix Text:**
```mariadb
Establish a process with accompanying tools for monitoring available disk space and ensuring that sufficient disk space is maintained to continue generating audit logs, overwriting the oldest existing records if necessary.
- To set up the audit logs to write to sylog:
- Edit the mariadb-enterprise.cnf file. Add the following under the [mariadb] section:
- server_audit_output_type = 'syslog'
- After the .cnf file is updated and saved, the mariadb database service must be restarted.
- If not writing to syslog, log rotation and purging should be configured.
```
### What type of control is being implemented?
### Todo is it set properly on your system?
---
### V-253754
### **Rule Title:**
MariaDB must generate audit records when security objects are modified.
### What is the Problem?
**Discussion:**
Changes in the database objects (tables, views, procedures, functions) that record and control permissions, privileges, and roles granted to users and roles must be tracked. Without an audit trail, unauthorized changes to the security subsystem could go undetected. The database could be severely compromised or rendered inoperative.
### What is the Fix?
**Fix Text:**
```mariadb
The MariaDB Enterprise Audit plugin can be configured to audit these changes.
- Update necessary audit filters to include query_event ALL. Example:
- MariaDB> DELETE FROM mysql.server_audit_filters WHERE filtername = 'default';
- MariaDB> INSERT INTO mysql.server_audit_filters (filtername, rule)
 VALUES ('default',
    JSON_COMPACT(
       '{
          "connect_event": [
             "CONNECT",
             "DISCONNECT"
          ],
          "query_event": [
              "ALL"
          ]
       }'
    ));
```
### What type of control is being implemented?
### Todo is it set properly on your system?
---


