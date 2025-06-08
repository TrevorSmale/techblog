+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 6 - Monitoring & Parsing Logs"
date = "2025-05-04"
description = "Monitoring & Parsing Logs"
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

Monitoring and parsing logs is essential to operational intelligence. Computers typically produce **immense** amounts of data—far more than a human can interpret in real time. To extract meaning from this data, we must intelligently filter event logs into clear, comprehensible, and actionable items.

Achieving this is easier said than done. This unit offers general advice on the art of making complex information comprehensible. [^1]

---

## Worksheet

### `Discussion Post 1`

Review chapter 15 of the SRE book:  
[^2]

There are 14 references at the end of the chapter. Follow them for more information. One of them by Julia Evans [^3]should be reviewed for question “c”.

`Question`

- What are some concepts that are new to you?

`Answer`

- Core dumps, Memory dumps, or Stack traces.

    I have heard the terms before and understand the concepts to a basic degree. I decided to do a bit of further reading to understand each of the dumps and traces so here is a gist.

    - **A core dump** is a snapshot of the processes state at the time of downing.
    - **A Memory dump** is a snapshot of the Random Access Memory (RAM) at the time of downing.
    - **A Stack trace** is the process of tracing function calls through the stack from end (error) to beginning (Call). The way I personally conceptualize this is through comparison to root cause analysis, something I am familiar with.


- **Host intrusion detection systems (HIDS)** or **Host Agents**

A few ideas from the book[^2]
"Modern (sometimes referred to as “next-gen”) host agents use innovative techniques aimed at detecting increasingly sophisticated threats. Some agents blend system and user behavior modeling, machine learning, and threat intelligence to identify previously unknown attacks."

"Host agents always impact performance, and are often a source of friction between end users and IT teams. Generally speaking, the more data an agent can gather, the greater its performance impact may be because of deeper platform integration and more on-host processing."

`Question`

- There are 5 conclusions drawn, do you agree with them? Would you add or remove anything from the list?

`Answer`

- To begin with, here are the conclusions drawn:

1. "Debugging is an essential activity whereby systematic techniques—not guesswork—achieve results."
2. "Security investigations are different from debugging. They involve different people, tactics, and risks."
3. "Centralized logging is useful for debugging purposes, critical for investigations, and often useful for business analysis."
4. "Iterate by looking at some recent investigations and asking yourself what information would have helped you debug an issue or investigate a concern."
5. "Design for safety. You need logs. Debuggers need access to systems and stored data. However, as the amount of data you store increases, both logs and debugging endpoints can become targets for adversaries."

Firstly, I would like to preface this answer with a disclaimer. I lack the competency to critisize and/or disect O'Relly's book. With that out of the way. I am going to target the first point.

My only criticism here is that the point is very broad in scope as compared to the more granular and topics specific to this book/chapter. 

`Question`

- In Julia Evan’s debugging blog, which shows that debugging is just another form of troubleshooting, what useful things do you learn about the relationship between these topics?

`Answer`

- Both debugging and troubleshooting involve:

- **Proceduralization**: If a clear procedure doesn’t exist, begin documenting and formalizing the process into a repeatable method.
- **Humility**: Acknowledge that you might be the cause of the problem. This is especially important in development.
- **Methodical Experimentation**: Form a hypothesis, then devise a controlled method to test it—use unit tests in development, or targeted scripts and commands when debugging.
- **One Step at a Time**: Tackle problems incrementally—“eat the elephant one bite at a time.”
- **Strong Foundations**: Write debuggable code and build robust systems. A good foundation makes issues easier to isolate.
- **More Is Better**: Verbose error messages provide more clues—enable detailed output when possible.

`Question`

- Are there any techniques you already do that this helps solidify for you?

`Answer`

Yes, I try to create excellent documentation with respect for my future self or others I may need to share it with. This involves numbered procedural steps with inputs and outputs, if that is the nature of the work. Otherwise, I write in a general manner that is legible to others.

### `Discussion Post 2`

Read Monitoring Distributed Systems [^4]

`Question`

- What interesting or new things do you learn in this reading? What may you want to know more about?

`Answer`

- Interesting Concept:

One of the general themes I gathered from this article is *low cognitive overhead*. It’s a concept I’m very familiar with from accessibility-focused design. Too much information overwhelms our ability to observe, absorb, and decide effectively.

For example, public signage must be simple, legible, and self-descriptive through clear graphic composition—guiding the eye where to look first and in which direction to proceed. This closely parallels the need for simplicity in monitoring and alerting systems. When such systems become overly complex, they can lead to misinterpretation, miscommunication, and fatigue due to information overload.

Information must be derived and presented in a way that is easily consumable, where errors are unmistakable—without exhausting the viewer.

New concepts

- White box monitoring systems vs. Black box monitoring systems.
- Conducting ad hoc retrospective analysis (ie. Debugging)
- (4 Golden signals) Latency, Traffic, Saturation, Errors
    - This one in particular relates strongly to the `USE` acronym I had recently picked up from Het, `Utilization, Saturation, Errors`

`Question`

- What are the “4 golden signals”?

`Answer`

1. Latency
2. Traffic
3. Saturation
4. Errors

`Question`

- After reading these, why is immutability so important to logging?

`Answer`

- **Tamper Resistance**: Immutable logs cannot be altered or deleted without detection, which helps prevent covering up malicious activity or mistakes.
- **Auditability**: Logs serve as historical records. If they can be changed, audits and investigations lose their value.
- **Debugging Integrity**: Developers and operators rely on logs to trace errors. Mutable logs can introduce false positives or hide root causes.
- **Regulatory Compliance**: Standards like HIPAA, PCI-DSS, and GDPR often require tamper-evident or immutable log storage.
- **Forensic Value**: In incident response, immutable logs serve as trustworthy evidence for timelines and breach analysis.

`Question`

- What do you think the other required items are for logging to be effective?

`Answer`

In order to be effective, log must be:

- **Trustworthy**: Logs should be **immutable**.
- **Time-stamped**: Every entry needs a synced timestamp.
- **Clear levels**: Use `INFO`, `ERROR`, `DEBUG`, etc., to show importance.
- **Structured**: Format logs so machines and humans can read them.
- **Context-rich**: Include request IDs, user info, IPs—anything that helps trace the story.
- **Centralized**: Gather logs in one place for easy searching and alerting.
- **Searchable**: You should be able to find issues fast with good queries.
- **Safe**: Control who can see logs—some contain sensitive info.
- **Durable**: Logs shouldn’t disappear in a crash—use backups and redundancy.
- **Noise-controlled**: Avoid flooding—rotate logs and cap log rates.

---

### Definitions

- `Application` Logs from software applications showing events or errors.
- `Host` Logs from the operating system, like kernel or authentication events.
- `Network` Logs capturing traffic, connections, and protocol activity.
- `DB` Logs from databases showing queries, errors, and access.
- `Immutable` Logs that cannot be changed once written.
- `RFC 3164 BSD Syslog` Older syslog format with simple priority and message structure.
- `RFC 5424 IETF Syslog` Modern syslog format with structured data and better timestamps.
- `Systemd Journal` Binary log format used by systemd with metadata support.
- `Log rotation` Archiving or deleting old logs to manage disk space.
- `Rsyslog` Advanced syslog daemon for filtering, formatting, and remote logging.
- `Log aggregation` Collecting logs from multiple sources for analysis.
- `ELK` Stack using Elasticsearch, Logstash, and Kibana to manage logs.
- `Splunk` Tool for searching and analyzing machine-generated logs.
- `Graylog` Open-source platform for central log collection and analysis.
- `Loki` Log system that indexes by labels, optimized for Grafana.
- `SIEM` Tools that collect and analyze security data for threat detection.

## Lab 🧪

### RSYSLOG

Reliable System and Kernel Logging System

Basic Steps:

1. Ensure Rsyslog is installed and running on both the control-plane and target node.
2. Configure sending of logs over UDP Port.
3. Editing Rsyslog config to split out logs.

`Question`

Why do we split out the logs in this lab? Why don’t we just aggregate them to one place?

`Answer`

- We are aggregating the logs.
- So that we can tell where the logs are coming from.  
- Each node is getting its own directory in `/var/log. 

`Question`

- What do we split them out by?

`Answer`

- We split them b y hostname.

`Question`

- How does that template configuration work?
    
`Answer`

- It will log to a specific directory named after the target hostname in `/var/log`.  

`Question`

- Are we securing this communication in any way, or do we still need to configure that?

`Answer`

- No we are not securing this communication, yes it needs further configuring.

`Lab`

- Complete the lab here: <https://killercoda.com/het-tanis/course/Linux-Labs/102-monitoring-linux-logs>

`Question`

- Does the lab work correctly, and do you understand the data flow?

- Yes 
1. Promtail (collects)
2. Loki (stores) 
3. Grafana (visualizes)


`loki-write.py`

`Question`

- Can you see it in your Grafana?

`Answer`

- Yes Scott is too awesome!

`Question`

- Can you modify the file loki-write.py to say something related to your name?

`Answer`

msg = 'On server {host} detected error - Treasure Wuz Here'.format(host=host)

`Question`

- Can you modify that to see the actual entires? 

`Answer`

- Yes

`Lab`

Complete the killercoda lab found here: <https://killercoda.com/het-tanis/course/Linux-Labs/108-kafka-to-loki-logging>

`Question`

- Did you get it all to work?

`Answer`

- Yes

`Question`

- Does the flow make sense in the context of this diagram?

`Answer`

- Yes
- Uses `kcat` to write out to Kafka
- Using promtail to receive the messages from kafka
- Promtail pushes to loki
- Displayed by grafana

`Question`

Can you find any configurations or blogs that describe why you might want to use this architecture or how it has been used in the industry?

`Answer`

- Kafka interconnects log `Producers` and log `Ingesters`. Kafka can ingest logs from all types of sources and analyze in real time.
- Apache Kafka, according to Apache, is the most popular open-source stream-processing software.[^5]

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 6 Worksheet [Web Book](https://professionallinuxusersgroup.github.io/psc/u6ws.html) ProLUG, 2025.
[^2]: Building Secure and Reliable Systems [Web Book](https://google.github.io/building-secure-and-reliable-systems/raw/ch15.html#collect_appropriate_and_useful_logs) Google, 2025.
[^3]: How to Debug [Blog](https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/ ) Julia Evans, 2019.
[^4]: SRE Handbook [Web Book](https://sre.google/sre-book/monitoring-distributed-systems/) Google, 2025.
[^5]: Powered by Kafka [Website](https://kafka.apache.org/powered-by/) Apache, 2025.


