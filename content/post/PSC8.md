+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 8 - Configuration Drift & Remediation"
date = "2025-05-11"
description = "Configuration Drift & Remediation"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

## Intro 👋

Configuration drift is the silent enemy of consistent, secure infrastructure.
When systems slowly deviate from their intended state, whether that be through manual changes, failed updates, or misconfigured automation, security risks increase and reliability suffers.[^1]

---

## Worksheet

### `Discussion Post 1`

Read about configuration management[^2]

`Questions`

What overlap of terms and concepts do you see from this week’s meeting?

`Answer`

- Lifecycle management and Change Control (Change Management). 
- Change Management is a system for ensuring process and product integrity.
- Despite these controls, variation from the norm (configuration drift) is inevitable.
- So we must invoke/involve controls in order to catch variation/drift.
- In the case of systems, it is bot Misconfigured Systems and Misconfigured Users to induce variation/drift.

`Question`

What are some of the standards and guidelines organizations involved with configuration management?

`Answer`

- Originally developed by the U.S. Department of Defense to ensure quality, reliability, and integrity in the manufacturing supply chain, configuration management principles were later adopted and expanded upon by standards bodies such as ANSI, ISO, and IEEE. These concepts have since evolved through industry-specific frameworks, including:

- ITIL
- ISO/IEC
- NIST
- IEEE
- CERN

`Question`

Do you recognize them from other IT activities?

`Answer`

- For sure. 

- `Baselining` Gathering telemetry from a system at its base config
- `Standards` Developing a standard for configuration or procedure to ensure consistent and predictable output
- `Controls` Controlling versions, changes, configurations
- `Automation` Automatic and Repeatable tasks
- `Variation` Departure from the standard
- `Remediation` Reconciliation, Correction, Rebasing

### `Discussion Post 2`

Review the SRE guide to treating configurations as code. Focus down on the “Practical Advice” section [^3] 

`Question`

- What are the best practices that you can use in your configuration management adherence?

`Answer`

- Don't Check in Secrets

- Make it Hermetic
    - Apply the Rigor of Code
    - Golden Image

- Make it Reproducible

    - Try to Implement a Software Bill of Materials (SBOM)
    - Patching (If warranted) records.

- Make it Verifiable
    - Binary Provenance
    - Use Signed Code
    - Verify Artifacts, Not Just People
    - Verifiable Build Architectures


`Question`

- What are the security threats and how can you mitigate them?

`Answer`

- Supply Chain Attacks
- Exposure of secrets
- Non-hermeticity and Drift
- Over-priveleging through automation
- Inadequate Auditing and Change Control
- Insecure Testing Environments
- Artifact Poisoning

`Question`

- Why might it be good to know this as you design a CMDB or CI/CD pipeline?

`Answer`

- The Pipeline is a major target. If something were to be malicious injected, the problem could propagate to all target platforms/devices.
- CMDB is a Source of Truth. A misconfiguration, bad record or malicious activity could invalidate hermeticity.
- Secrets and Credentials flow through the Pipeline, a whole can of worms.

### Definitions

- `System Lifecycle` The full span of a system's life: design, build, operate, maintain, and retire.
- `Configuration Drift` The divergence of a system’s current state from its intended or documented configuration.
- `Change management activities` Processes that control changes to systems to reduce errors and downtime.
- `CMDB` (Configuration Management Database) A database tracking system components and their relationships.
- `CI` (Configuration Item) Any component in the CMDB (e.g., server, software, network) being tracked and managed.
- `Baseline` A known good configuration state used for comparison and control.
- `Build book` A documented set of steps to initially install and configure a system.
- `Run book` A manual or automated guide for maintaining or operating a system post-deployment.
- `Hashing` The process of generating a fixed-size value from data to verify integrity.
- `md5sum` Tool that calculates a 128-bit MD5 hash for checking file integrity.
- `sha<x>sum` Tools (e.g., `sha256sum`) that generate SHA-family hashes for stronger integrity checks.
- `IaC` (Infrastructure as Code) Managing infrastructure using versioned code instead of manual processes.
- `Orchestration` Coordinating automated tasks across multiple systems or services.
- `Automation` Replacing manual tasks with scripts or tools to increase speed and consistency.
- `AIDE` (Advanced Intrusion Detection Environment) A file integrity checker that detects unauthorized changes.

### Lab 🧪

#### STIG Viewer – Change Management

`Question`
- How many STIGs relate to "change management" in RHEL 9?

`Answer`
- 9 STIGs contain the phrase.

`Question`
- What does a "robust change management process" imply?

`Answer`
- Change control, peer review, versioning, testing, and approval are mandatory before config updates.

`Question`
- Can one STIG enforce this?

`Answer`
- No, it's an org-wide practice beyond simple config toggles.

`Question`
- What type of control is applied?

`Answer`
- Technical preventative—mostly file ownership/permissions.

`Question`
- Are they all the same?

`Answer`
- Yes, the control type is consistent across them.

#### Monitoring Configuration Drift with AIDE

`Question`
- What is `/etc/aide/aide.conf.d/`?

`Answer`
- Contains rule files defining paths to hash and monitor.

`Question`
- How many files are there?

`Answer`
- 213 files.

`Question`
- What does `aide -v` show?

`Answer`
- Version 0.18.6

`Question`
- What is AIDE?

`Answer`
- File integrity checker using stored hashes in a database.

`Question`
- What does `/etc/cron.daily/dailyaidecheck` do?

`Answer`
- Runs `dailyaidecheck` via `capsh` if available, otherwise with `bash`.

`Question`
- What does `capsh` do?

`Answer`
- Launches processes with limited capabilities—safer than full root.

`Question`
- What does `aide -i` do?

`Answer`
- Initializes the DB. It took ~4m14s. User time was ~3m30s.

`Question`
- Why track timing?

`Answer`
- For planning and resource estimation during mass deployments.

`Question`
- What’s in the output?

`Answer`
- Hashes (MD5, SHA, etc.) and `/var/lib/aide/aide.db.new`.

`Question`
- What should you study?

`Answer`
- RMD160, TIGER, CRC32, HAVAL, WHIRLPOOL, GOST.

#### AIDE Test Run

`Question`
- What’s the test procedure?

`Answer`
- Create `/root/prolug/test*`, run aide check.

`Question`
- Were files detected?

`Answer`
- Yes, under "Added entries."

`Question`
- Runtime?

`Answer`
- ~6m38s, user ~5m54s, sys ~8s.

#### Remediating Drift with Ansible

`Question`
- What does the web env lab do?

`Answer`
- Deploys 3 virtual hosts (dev, test, qa) on ports 808{0,1,2}.

`Question`
- How do you test?

`Answer`
- `curl node01:808{0,1,2}`

`Question`
- What happened to 8081?

`Answer`
- It failed initially—intentional drift.

`Question`
- Does re-running the playbook fix it?

`Answer`
- Yes, restores state without manual steps.

`Question`
- Will that always work?

`Answer`
- Yes, unless networking/firewall issues prevent access.

`Question`
- Can this cause issues?

`Answer`
- Yes, if configs were changed manually after deployment.

`Question`
- Root cause: tech or ops?

`Answer`
- Operational—teams must coordinate changes.

#### Challenge: Custom Reporting

`Question`
- How would you verify stamp compliance?

`Answer`
- Use Ansible facts and add deployment date as a custom variable.


### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 8 [Web Book](https://professionallinuxusersgroup.github.io/psc/u8intro.html) ProLUG, 2025.
[^2]: Configuration Management [Wiki](https://en.wikipedia.org/wiki/Configuration_management) Wikipedia, 2025.
[^3]: Building Secure and Reliable Systems [Web Book](https://google.github.io/building-secure-and-reliable-systems/raw/ch14.html#treat_configuration_as_code) Google, 2025.
[^4]: Telemetry [Web Book](https://grafana.com/docs/tempo/latest/introduction/telemetry/) Grafana, 2025.


