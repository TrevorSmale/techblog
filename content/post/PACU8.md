+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 8 🐧"
date = "2024-10-30"
description = "ProLUG Admin Course Unit 8"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Scripting System Checks

![Arnold](https://trevorsmale.github.io/techblog/images/PACU8/ibb.jpg)

Once again beyond the Discussion Posts and Labbing. I spent a great deal of time scripting/programming System checks. After completing the labs which Bash Scripting and intro to 'C', I got really into GO as a system util. I have a particularly productive day with using the embed.fs feature of GO and packing unix system tools together in a single go program at compilation. I think there is a ton of potential here for my own uses. 👨‍🔧

## Discussion Post 1 

Scenario

It’s a 2-week holiday in your country, and most of the engineers and architects who designed the system are out of town. You’ve noticed a pattern of logs filling up on a set of web servers due to increased traffic. Research and verification show that the logs are being sent off in real time to Splunk. Your team has been deleting the logs every few days, but a 3rd-shift engineer missed this in the notes, causing downtime. How might you implement a simple fix to stop-gap the problem until all engineering resources return next week?

- **Resources Used**:

- TryHackMe (Splunk) Intro
- Study Group discussion
- ChatGPT
- Blogs:
- [Nohl's Tech Resources: Log Files in tmpfs Without Breaking Logging](https://www.nohl.eu/tech-resources/notes-to-linux/log-files-in-tmpfs-without-breaking-logging/)
- [DietPi Documentation: Log System](https://dietpi.com/docs/software/log_system/)

#### Why can’t you just make a design fix and add space in /var/log on all these systems?

Adding more space to `/var/log` might be a design fix, but it isn’t feasible in the short term due to:

- **Operational Constraints**: Extending storage may involve downtime, additional permissions, or architectural changes that can’t be approved without the primary engineers.
- **Temporary Nature of the Fix**: Increasing space only delays the issue. If logs continue to grow, the problem will recur once space is exhausted again.

#### Why can’t you just make a design change and use logrotate more frequently?

- **Possibility of Log Loss**: Higher logrotate frequency could still miss high-frequency log spikes, especially during unusual traffic peaks, risking logs being deleted before Splunk ingestion is complete.
- **Configuration and Testing**: Aggressive logrotate adjustments may interfere with processes expecting logs at specific retention periods. Testing changes in production without key team members isn’t ideal.

#### Temporary Fix Options

To address the issue, consider implementing a temporary fix by configuring a log retention policy that aggressively compresses or truncates logs without disrupting active processes. Here are some potential approaches:

### Implement a Temporary **Cron Job**

Schedule a cron job to truncate logs on a more aggressive schedule without deleting them. For example:

```bash
> /var/log/access.log

```bash

  > /var/log/access.log

```

This would empty the log file without removing it or impacting the active file descriptors held by any running processes.

### Set Up Temporary Log Compression

Compress the logs after truncation if additional space savings are needed. Tools like gzip can compress logs efficiently, reducing disk space usage and ensuring logs are still accessible if required for audits or incident investigations.

### Implement a RAM Disk for Temporary Logs

As a short-term measure, you could set up a RAM disk for logs that don’t need long-term retention. This allows logs to be stored temporarily in memory, reducing disk space pressure. For instance

```bash

  mount -t tmpfs -o size=512M tmpfs /var/log/temp

```

You could then configure lower-priority logs to write here temporarily, knowing they will be lost upon reboot, which may be acceptable in a crisis scenario.

### Adjust Splunk Forwarder Configuration:

If possible, configure the Splunk forwarder to filter logs more aggressively, reducing the volume of logs that are retained on the system. The props.conf or inputs.conf files can be configured to forward logs without keeping local copies.


Adding more space to /var/log might be a design fix, but it isn’t feasible in the short term due to the following:
- Operational Constraints: Extending storage could involve downtime, additional permissions, or changes that require architectural decisions that can’t be made without the primary engineers.
- Temporary Nature of the Fix: Increasing space only delays the issue rather than preventing it. If the logs keep growing, the problem will recur once space runs out again.

## Discussion Post 2 

You are the only Linux Administrator at a small healthcare company. The engineer/admin before you left you a lot of scripts to untangle. This is one of our many tasks as administrators, so you set out to accomplish it. You start to notice that he only ever uses nested if statements in bash. You also notice that every loop is a conditional `while true` and then he breaks the loop after a decision test each loop. You know his stuff works, but you think it could be more easily written for supportability, for you and future admins. You decide to write up some notes by reading some google, AI, and talking to your peers.

### Compare the use of nested if versus case statement in bash.

- Nested if statements are useful for situations where each condition depends on the result of the previous test, requiring a hierarchy or sequence.
- A case statement is ideal for handling multiple discrete values of a variable, especially if there are many possible branches. It’s typically cleaner and more readable than a nested if.

### Compare the use of conditional and counting loops. Under what circumstances 

- Use conditional loops (while) when you don’t know the number of iterations in advance and need to loop based on conditions.
- Use counting loops (for) when you have a set number of iterations or are working with a list. This structure is clearer and prevents issues that may arise from unintentional infinite loops.

### would you use one or the other?

optimizing or refactoring Bash scripts the Engineer had left me.

- I would replace nested if statements with case statements when possible to improve readability, especially when handling multiple discrete values.
- Of course, I would comment things for added communication/ maintainability.
- I would Limit while true loops to cases where no predictable count or list is available. Clearly define a break condition early to avoid infinite loops.
- I would Use for loops for counting or iterating over arrays or lists, as they provide a clean structure with known iteration limits.


---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis


[^1]: MITRE | ATT&CK [Site](https://attack.mitre.org/) Knowledge Base.