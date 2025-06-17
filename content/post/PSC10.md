+++
author = "Hugo Authors"
title = "ProLUG SEC Unit 10 🔒"
date = "2025-06-17"
description = "ProLUG Security Engineering Course Capstone Project"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Security"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

## Intro 👋

This is the final Unit and close to the Linux Security Course. Though we did not have labs for this Unit, we did spend a lot of time reflecting.

## Closing Thoughts

I recently finished the first ProLUG Security Engineering Course, designed and delivered by Scott Champine, also known as Het Tanis, from ProLUG. It ran for about 10 weeks and clocked in at roughly 100 hours of focused effort—but honestly, I probably put in more than that once you count the spontaneous study sessions and the many side discussions that came up. A small group of us showed up consistently and really dug into the material, connecting ideas and bouncing thoughts off each other.

The course itself was free and not tied to any official institution, but it was taught by a seasoned industry professional who also teaches at the post-secondary level. Scott clearly cares about the subject and about helping others understand it. That came through in how he delivered the material, and it brought out a real sense of commitment in us too.

On top of just taking the course, I also helped shape it for future learners by starting a version-controlled course book. We had a small group that met weekly to go over edits and review pull requests. A few people even joined just to learn Git so they could contribute, which added to the sense of shared effort and made the experience even better.

One of the things that helped me stay on track was having a study group. There’s a lot of sharp, motivated people in the ProLUG community, and quite a few of them kept up a steady pace through both the course and the book. The regular check-ins and shared discussions made a big difference.

The course itself covered a wide range of topics and gave me a stronger sense of how enterprise security is put together, maintained, and kept resilient. Security isn’t just about ticking boxes—it touches every part of a system. Especially with Linux, where multiple users and external inputs are constantly in play, it doesn’t take much for something to go sideways if you’re not paying attention.

We worked through the process of hardening Linux systems using STIGs—basically long, detailed lists of potential vulnerabilities and how to guard against them. It’s not fast work, but it really forces you to think about each configuration choice.

Patching was another major topic, and not in the usual “just update it” way. We talked about how every change introduces risk, and how important it is to approach patching as part of a controlled, planned process. That includes things like internal repositories, known-good system images, and minimizing surprise behavior from updates.

We also got hands-on with locking down systems: managing ingress and egress, shutting off unnecessary ports, setting up bastion hosts, and building out logging and alerting. We even worked on ways to trap misbehaving users or bots inside chroot jails. One of the others in the group even automated that process with a Bash script for their final project.

We had deep conversations about monitoring too—things like how to design alerts that people can actually respond to, without burning out from constant noise. We looked at log filtering, storage, and what makes a log useful rather than just more clutter.

We also talked about automation and how it can sometimes get away from you. It’s easy for parts of a system to drift out of spec if you’re not careful, especially with orchestration tools. So we looked at how to use infrastructure-as-code and version control to make changes traceable and systems more predictable.

Toward the end of the course, we focused on trust, keys, and certificates. We got practical—generating and managing key pairs, breaking them, fixing them, and eventually building up to TLS certificates. These exercises helped drive home how trust is managed inside systems, especially in setups that lean toward zero trust.

Before this course, I already had a decent background in cybersecurity—some labs, a few certifications—but this gave me something more solid. I now feel like I understand what it means to build security into a system, rather than just bolt it on. I’m more confident setting up and maintaining a hardened Linux environment, and more thoughtful about how to track and manage change over time.

That said, I don’t think I’ve “arrived.” If anything, this course just made me more aware of how much I still have to learn. I’ve moved into that space where I know what I don’t know, and that’s a valuable place to be. It’ll take years to keep digging through it all, but now I’ve got a better starting point—and the confidence to figure things out when new challenges come up.

All in all, this course gave me a deeper appreciation for operational security, and it left me with some solid tools I’ll continue to use. Like with the Admin course before it, I really valued the people I got to work with. I expect we’ll keep exploring these topics together for a long time. And, like always, you make a few good friends along the way.

## Discussion Post 1

`Question`

- How many new topics or concepts do you have to go read about now?

`Answers`

- `TLS` Transport Layer Security: Prior to the course, I was aware of the terminology and had a 30,000 Foot conceptual view. During this course, I was able to zoom in an take a look at the transport and the layers. However, given the shear scale and complexity of the topic. I will have to read through the `1.1`, `1.2` and `1.3` Specifications. One of my favorite IT Authors Michael W. Lucas has a book for sale on the topic. https://www.tiltedwindmillpress.com/product/tls/

- `ZT` Zero Trust: I get from a high level view as well. I learned that Zero Trust is a popular buzz-word or a form of jargon for most. Actually drilling down and understanding the many forms and configurations of a `ZT` network is an immense undertaking. On the side I had done some additional reading about it, for example, I read through some of https://www.cisa.gov/sites/default/files/2023-04/CISA_Zero_Trust_Maturity_Model_Version_2_508c.pdf
and plan to dig deeper into the subject.

- `Tokenization` & `Data Masking` are two interesting topics. If anyone can recommend materials, I am interested. So far I have just found Wikipedia for the explanations.

- `SSO` Single Sign On: I found this book: https://fw2s.com/wp-content/uploads/2017/09/definitive-guide-to-single-sign-on.pdf

- `DMARC` Domain-based Message Authentication Reporting & Conformance: I do have the book `Run your own mail server` by Michael W. Lucas. I am assuming he covers the topic to some degree.

- `SPF` Sender Policy Framework: Which should be covered by the aforementioned book as well.

- `CVSS` Common Vulnerability Scoring System: I found this paper on the matter, it is a technical specification. https://www.first.org/cvss/v3-1/cvss-v31-specification_r1.pdf

- `TSDB's` Time Series Databases: I have heard the concept from this course and in game development. I think the concept is easy to grasp. But I would like to investigate further.

`Question`

What was completely new to you?

`Answer`

- `STIGS` for sure. Just prior to starting the course, I was given a glimpse of the Stig'ing process. In the course we were tasked with getting the StigViewer working, downloading specific STIG's and Implementing hardening while answering prompts about what specifically we were doing. It really helped to have finished the admin course prior to this as it made the objectives more clear to me.

- `Bastions Hosts` Prior to using the one implemented on Scott' own server, I had not seen this. Drilling into the concept and creating a Bastion in the lab was a nice intro.

`Question`

- What is something you heard before, but need to spend more time with?

`Answer`

- I had heard the Acronyms for many of the concepts prior to this course. In my answer to `Question #1`, I had already detailed what I will need to dig into after the course is complete.

---

## Discussion Post 2

`Scenario`

- Think about how the course objectives apply to the things you’ve worked on.

`Question`

- How would you answer if I asked you for a quick rundown of how you would
secure a Linux system?

`Answer`

- First, I’d check open ports using `ss -ntulp` to see what services are listening and close anything unnecessary.
- Next, I’d check how many user accounts exist by running `cat /etc/passwd | wc -l`, and optionally review users with high UIDs to see who has real login access.
- I’d confirm that root login over SSH is disabled by checking `/etc/ssh/sshd_config` and setting `PermitRootLogin no`.
- Then I’d check for any accounts with empty passwords using `awk -F: '($2 == "") { print $1 }' /etc/shadow`.
- I’d list which users have sudo access by checking the `sudo group` or reviewing `/etc/sudoers`.
- I would review running services with `systemctl list-units --type=service` and disable anything that isn’t needed.
- Then I’d make sure a firewall is enabled and configured, using `firewalld`, `ufw`, or `iptables`, depending on the system.
- I’d update all packages using the system’s package manager like dnf, apt, or yum to ensure known vulnerabilities are patched.
- I’d also check file permissions on sensitive files like `/etc/shadow` and `/home/* directories`.
- If SSH is exposed, I’d install and configure `fail2ban` to protect against brute-force login attempts.
- I’d regularly check system logs like /var/log/auth.log or use `journalctl` to spot anything suspicious.
- Lastly, I’d run a tool like Ansible Lock-Down to audit and find common misconfigurations.

`Question`

- How would you answer if I asked you why you are a good fit as a security
engineer in my company?

`Answer`

Though I am not a seasoned Security Engineer, I possess a solid understanding of Linux, system hardening, and monitoring techniques, along with a strong foundation in high-level concepts related to ensuring security, reliability, and confidentiality in systems and networks. I am a diligent learner and a prolific documenter, always striving to deepen my knowledge and contribute meaningfully to operational resilience and security best practices.

`Frame`

- Think about what security concepts you think bear the most weight as you
put these course objectives onto your resume.

`Question`

- Which would you include?

`Answer`

I would perhaps list generalities

- Linux System Security Auditing
- Linux System Hardening
- Linux System Monitoring
- Linux System Access Control 
- Encryption & Certificate Management
- Infrastructure Security Governance Compliance

`Question`

Which don’t you feel comfortable including?

`Answer`

- Network Security
- Transport Layer Security


### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis




