+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 7 🔒"
date = "2025-05-11"
description = "Monitoring & Alerting"
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

Monitoring systems and alerting when issues arise are critical responsibilities for system operators. Effective observability ensures that system health, performance, and security can be continuously assessed.[^1]

---

## Worksheet

### `Discussion Post 1`

Intro to the scenario

Read about telemetry, logs, and traces[^2][^4]. 

`Question`

- How does the usage guidance of that blog align with your understanding of these three items?

`Answer`

Though the concepts involved in telemetry are really quite simple, they took me some time to internalize and fully understand.
I can't say it paralleled my own understanding as my understanding was very limited. Prior to the lectures, if I were to hear the word telemetry, I would think of non GPS tracking techniques or some sort of secret tracking by Palantir.

My simplified outline of these 3 things:

- A metric represents a point in time measurement of a particular source
- Logs are discrete and event triggered occurrences.
- Traces follow a program’s flow and data progression.

`Question`

- What other useful blogs or AI write-ups were you able to find?

`Answer`

- Yes Grafana has a good writeup on the subject.
- https://grafana.com/docs/tempo/latest/introduction/telemetry/

`Question`

- What is the usefulness of this in securing your system?

`Answer`

- Securing a System is useful in many ways. 
- Prevents unwanted access.
- Potentially mitigates Data Exfiltration.
- Potentially mitigates unwanted Data Infiltration.
- Prevents miss-use by users.
- Can simply mitigate the overload of available resources.
- Not only does the attack surface shrink when a system is properly secured, but the monitoring tasks reduce.
- Secure systems are more predictable systems.

### `Discussion Post 2`

Intro to the scenario

When we think of our systems, sometimes an airgapped system is simple to think about because everything is closed in. The idea of alerting or reporting is the opposite. We are trying to get the correct, timely, and important information out of the system when and where it is needed.

Read the summary at the top[^3]. 

`Question`

- What is the litmus test for a page? (Sending something out of the system?)

`Answer`

- The page must be pertaining to an imminent, actionable situation that must be addressed quickly. 

`Question`

- What is over-monitoring v. under-monitoring. Do you agree with the assessment of the paper? Why or why not, in your experience?

`Answer`

- Over-monitoring can be compared to hyper-vigilance. Over time it works against you as fatigue or indifference sets in. Furthermore over-monitoring includes the transporting, receiving and dissemination of too much information, causing cognitive overload, leading to poor decision making. Furthermore, the additional information being broadcast leaves a system more susceptible/vulnerable from a security stand-point. 

- Under monitoring would be lack of contextual reporting, responsiveness and diligence needed in order to keep a system from going down. 

- From reading this article, it seems to me that one must turn monitoring into a spectrum of detail. Hypercritical indicators like uptime, load, capacity should be reported daily with a pre-determined baseline. Estimations can be made from this allowing for prediction. While major changes --those outside the predicted norm, could trigger alerts. Paging should be reserved for the utmost critical issues.

`Question`

- What is cause-based v. symptom-based and where do they belong? Do you agree?

`Answer`

- Cause based is the analysis/investigation of the root cause of a certain outcome. In the context of systems operating and security, it is finding the vulnerability, infiltration/exfilration point or cause of failure like hitting memory/cpu/disc space limitations.

- Symptom based analysis would be to observe the effects of an unknown origins ie. systems going down, data loss etc...
Bringing the system back up, or restoring data from backups does nothing to address the root cause, it only remediates the effects.

---

### Definitions

- `Telemetry` Automated collection of system metrics and status data.
- `Tracing` Tracks the path and performance of requests across services.
- `Span` A single unit of work in a trace, with start and end timestamps.
- `Label` Key-value pair used to add metadata to traces or metrics.
- `Time Series Database (TSDB)` A database optimized for storing data indexed by time.
- `Queue` A data structure or service for holding and processing messages in order.
- `UCL/LCL` Statistical limits used to detect anomalies in metrics over time.
- `Aggregation` Combining multiple data points into a summarized form.
- `SLO` Service Level Objective, a target performance metric.
- `SLA` Service Level Agreement, a contractually agreed service standard.
- `SLI` Service Level Indicator, a specific measurement of system performance.
- `Push` Data sent actively from source to receiver.
- `Pull` Data requested by the receiver from the source.
- `Alerting rules` Conditions set to trigger alerts based on system metrics.
- `Alertmanager` Tool for handling, deduplicating, and routing alerts.
- `Alert template` Format used to display or notify alert information.
- `Routing` Directing alerts to specific teams or destinations.
- `Throttling` Limiting the number or rate of alerts to reduce noise.
- `Monitoring for defensive operations` Watching systems for signs of attack or failure.
- `SIEM` Centralized platform for analyzing security and event logs.
- `IDS` Tool that detects suspicious or unauthorized activity.
- `IPS` Tool that blocks or prevents detected threats in real time.

## Lab 🧪

## Fail2Ban Setup and Testing

### Install Fail2Ban

- Install Fail2Ban:
  ```bash
  apt install -y fail2ban
  ```

### Verify Installation

- Check the version of Fail2Ban:
  ```bash
  fail2ban-client --version
  ```

### Configure SSHD Jail

- Edit the jail configuration file:
  ```bash
  vi /etc/fail2ban/jail.conf
  ```

- Uncomment `[sshd]` and add the following under that section:
  ```ini
  [sshd]
  enabled = true
  maxretry = 5
  findtime = 10
  bantime = 4h
  ```

- Review the rest of the file and **ensure there is no duplicate `[sshd]` section**. Comment it out or remove it if found.

### Explore Other Jails

- Review configuration sections for Apache and NGINX to see other available jails.

### Restart and Verify Fail2Ban

- Restart the service:
  ```bash
  systemctl restart fail2ban
  ```

- Check status:
  ```bash
  systemctl status fail2ban --no-pager
  ```

### Test the SSH Ban

- SSH into `node01`:
  ```bash
  ssh node01
  ```

- Run a loop to simulate failed login attempts:
  ```bash
  for i in {1..6}; do ssh invaliduser@controlplane; done
  ```

- Press `Enter` on each password prompt until Fail2Ban triggers.
- Use `Ctrl + C` to exit the loop when connection attempts are blocked.

### Check Ban Status

- Return to `controlplane`.

- View the logs:
  ```bash
  tail -20 /var/log/fail2ban.log
  ```

- Check banned IPs:
  ```bash
  fail2ban-client get sshd banned
  ```

`Question`

- Do you see the expected IP address in the ban list?
- Why do you think that is?

`Answer`

- Yes I was able to see it.

### Unban the IP

- Replace `<IP>` with the actual banned IP:
  ```bash
  fail2ban-client set sshd unbanip <IP>
  ```

### Confirm Unban

- SSH into `node01`:
  ```bash
  ssh node01
  ```

- Try to reconnect to `controlplane` using the correct user:
  ```bash
  ssh root@controlplane
  ```

`Question`

- Did the connection succeed?

`Answer`

- Yes, I was able to reconnect


### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 7 [Web Book](https://professionallinuxusersgroup.github.io/psc/u7intro.html) Source, 2025.
[^2]: Observability Chapter [Web Book](https://microsoft.github.io/code-with-engineering-playbook/observability/log-vs-metric-vs-trace/) Source, 2025.
[^3]: My Philosophy on Alerting [Google Doc](https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit?tab=t.0) Rob Ewaschuk, 2014.
[^4]: Telemetry [Web Book](https://grafana.com/docs/tempo/latest/introduction/telemetry/) Grafana, 2025.


