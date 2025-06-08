+++
author = "Hugo Authors"
title = "ProLUG Security Engineering Course Unit 5"
date = "2025-04-27"
description = "Repos & Patching"
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

Repositories and Patching is the general theme of this unit. We dive into creating internally audited repositories for safe enterprise operation. This configuration allows for greater security scrutiny and compatibility testing before schedule patching takes place. For example, a company would like to skip every other version of a package in order to reduce update cadence, giving more time for assessment, correction and troubleshooting of internal software. Much like any enterprise decision regarding cost and effort and analysis must take place. [^1]

---

## Worksheet

### `Discussion Post 1`

Review the rocky documentation on Software management in Linux.[^2]

`Question`

- What do you already understand about the process?

`Answer`

- I had gained a decent understanding of the package management systems of both RHEL and DEBIAN based distros through both studying for the LPIC 1 and completing the ELAC course through this group. From this I had learned about versioning and dependency management including modules. The differences between RPM, YUM, and DNF and how these evolutions of package management came into being.

`Question`

- What new things did you learn or pick up?

`Answer`

- - I did not understand the depth to which RPM packages tracked package data.
- I did not know that headers could be edited in the package in order to add custom labeling.
- I had basic awareness and high level understanding of internal package management. Prior to our lecture, I had not seen an internal package server/relay be setup/configured.
- I did not know much about EPEL beyond having to call it in the CLI for additional packages outside of DNF.

`Question`

- What are the DNF plugins? What is the use of the versionlock plugin?

`Answer`

- DNF plugins are external modules that extend the functionality of DNF. I have some experience activating the `COPR` repo using DNF plugins. Furthermore, `versionlock` is a specific plugin that allows for an admin/engineer/dev to lock a particular package to a specified version so that it is not mistakenly changed/overwritten. This is typically done in software development in my experience, with software development many dependencies might be needed. Breaking updates were common place, so most modern software projects contain a `lock` file that indicates what specific dependency version must be used in order to build or interpret the project.

`Question`

- What is an EPEL? Why do you need to consider this when using one?

`Answer`

- `EPEL` stands for Extra Packages for Enterprise Linux. These packages exist outside of the core enterprise offering and are therefore potentially issue causing. Unlike core packages, these extra packages could introduce possible incompatibility issues, resulting in rejection by endorsed support specialists.

### `Discussion Post 2`

Do a google search for “patching enterprise Linux”[^3]

`Question`

- What blogs (or AI) do you find that enumerates a list of steps or checklists to consider?

`Answer`

- I used to sources that I had found to be concise
  - RedHat Documentation[^4]
  - Chat*ippity

`Question`

- After looking at that, how does patching a fleet of systems in the enterprise differ from pushing “update now” on your local desktop? 

`Answer`

- Patching a fleet of systems involves the systematic updating of installed software to fix security vulnerabilities, improve stability, and introduce minor enhancements. The process is governed by organizational policies to ensure uptime and compliance.

Because changes affect many systems simultaneously, patching acts as an amplifier of problems if not handled carefully. Therefore, enterprise patching must be strategic, managed, and auditable.

In contrast, running updates on a personal system is typically an automated, low-risk operation, with little concern for version conflicts or trust in the source. Additionally, modern filesystems like ZFS and Btrfs provide the ability to quickly roll back changes if something fails.

`Question`

- What seems to be the major considerations? What seems to be the major roadblocks?

`Answer`

- Major Considerations
  - Uptime and Service Continuity
  - Security and Compliance
  - Testing and Validation
  - Dependency Management
  - Rollback and Recovery Planning
  - Orchestration and Scalability

- Major Roadblocks
  - Legacy Systems
  - Change Resistance
  - Incomplete Asset Inventory
  - Tight Maintenance Windows
  - Patch Quality and vendor Bugs
  - Complex Dependencies and Integration Points
  - Resource Constraints

---

### Definitions

- `Patching` The process of applying updates to fix bugs, improve security, or enhance performance.
- `Repos` (Repositories) Remote or local collections of software packages used by package managers.
- `Software` Applications or tools installed on a system to perform specific functions.
- `EPEL` (Extra Packages for Enterprise Linux) A Fedora project providing additional packages for RHEL-based systems.
- `BaseOS` The core operating system components in RHEL/Rocky, including the kernel and essential services.
- `AppStream` A modular repository in RHEL/Rocky that provides applications and tools in versioned streams.
- `httpd` The Apache HTTP Server package available via repos for web serving.
- `patching` A type of update package that modifies existing software without replacing the whole binary.
- `GPG Key` Used to verify the integrity and authenticity of packages in a repo.
- `DNF/YUM` Package managers in RHEL-based systems used to install, update, and manage software packages.

## Lab 🧪

### Apache STIGs Review

1. Look at the 4 STIGs for “tls”
  - What file is fixed for all of them to be remediated?

```bash
# Install httpd on your Rocky server
systemctl stop wwclient
dnf install -y httpd
systemctl start httpd
```

2. Check STIG V-214234

`Question`

- What is the problem?

`Answer`

- Event logging can fail.

`Question`

- What is the fix?

`Answer`

- This can be fixed by implementing failure alerts.

`Question`

- What type of control is being implemented?

`Answer`

- This type of control would be a `Detective Control`

`Question`

- Is it set properly on your system?

`Answer`

- No, this is not setup by default, it must be implemented after installation.

Check STIG V-214248

`Question`

- What is the problem?

`Answer`

- By default, sensitive information including security controls may be available to all users because privelaged user access controls have not been implemented.

`Question`

- What is the fix?

`Answer`

- Develop roles for privelaged users and define access policies.

`Question`

- What type of control is being implemented?

`Answer`

- This is a preventative type control.

`Question`

- Is it set properly on your system?

`Answer`

- No, not by default. Of course super user has special priveledges. However, beyond that there are no other tiers of access.

`Question`

- How do you think SELINUX will help implement this control in an enforcing state? Or
will it not affect it?

`Answer`

- SELINUX allows for strong group creation and control. So it would help batch users and apply granular control mechanisms.

---

### Building repos

```bash
# Start out by removing all your active repos
cd /etc/yum.repos.d
mkdir old_archive
mv *.repo old_archive
dnf repolist
```

```bash
# Mount the local repository and make a local repo
mount -o loop /lab_work/repos_and_patching/Rocky-9.5-x86_64-dvd.iso /mnt
df -h #should see the mount point
ls -l /mnt
touch /etc/yum.repos.d/rocky9.repo
vi /etc/yum.repos.d/rocky9.repo
```

```rocky9.repo
[BaseOS]
name=BaseOS Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
[AppStream]
name=AppStream Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
#Save with esc :wq or “shift + ZZ”
```

`Question`

- Do the paths you’re using here make sense to you based off what you saw
with the ls -l? Why or why not?

`Answer`

- TODO

```bash
chmod 644 /etc/yum.repos.d/rocky9.repo
dnf clean all
# Test the local repository
dnf repolist
dnf --disablerepo="*" --enablerepo="AppStream" list available
Approximately how many are available?
dnf --disablerepo="*" --enablerepo="AppStream" list available | nl
dnf --disablerepo="*" --enablerepo="AppStream" list available | nl | head
dnf --disablerepo="*" --enablerepo="BaseOS" list available
Approximately how many are available?
dnf --disablerepo="*" --enablerepo="BaseOS" list available | nl
dnf --disablerepo="*" --enablerepo="BaseOS" list available | nl | head
# Try to install something
dnf --disablerepo="*" --enablerepo="BaseOS AppStream" install gimp
hit “n”
```

`Question`

- How many packages does it want to install?

`Answer`

- TODO

`Question`

How can you tell they’re from different repos?

`Answer`

- TODO

```bash
# Share out the local repository for your internal systems (tested on just this one system)
rpm -qa | grep -i httpd
systemctl status httpd
ss -ntulp | grep 80
lsof -i :80
cd /etc/httpd/conf.d
vi repos.conf
```

```repos.conf
<Directory "/mnt">
Options Indexes FollowSymLinks
AllowOverride None
Require all granted
</Directory>
Alias /repo /mnt
<Location /repo>
Options Indexes FollowSymLinks
AllowOverride None
Require all granted
</Location>
```

```bash
systemctl restart httpd
vi /etc/yum.repos.d/rocky9.repo
```

```rocky9.repo
###USE YOUR HAMMER MACHINE IN BASEURL###
[BaseOS]
name=BaseOS Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
#baseurl=file:///mnt/BaseOS/
baseurl=http://hammer25/repo/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
[AppStream]
name=AppStream Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
#baseurl=file:///mnt/AppStream/
baseurl=http://hammer25/repo/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```

`Question`

- Do the paths you’ve modified at baseurl make sense to you? If not, what do you need to better understand?

```bash
dnf clean all
dnf repolist
# Try to install something
dnf --disablerepo="*" --enablerepo="BaseOS AppStream" install gimp
```

### Digging Deeper

`Question`

- You’ve set up a local repository and you’ve shared that repo out to other systems that might want to
connect. Why might you need this if you’re going to fully air-gap systems? Is it still necessary even if
your enterprise patching solution is well designed? Why or why not?

`Answer`

- We need a unified checkpoint that ensures secure conformity of a package before patching air-gapped systems. Air-gapped systems are not eternally disconnected, they can be connected to other systems in a highly controlled manner.

`Question`

- Can you add the Mellanox ISO that is included in the /lab_work/repos_and_patching section to be a
repository that your systems can access? If you have trouble, troubleshoot and ask the group for
help.

`Answer`

- Yes you can, it must be given a special header and be registered aka. Packaged into the local repo in order for other package management systems to see it.

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG PSC Repo: https://github.com/ProfessionalLinuxUsersGroup/psc
ProLUG PSC Book: https://professionallinuxusersgroup.github.io/psc/
ProLUG Book of Labs: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

---

[^1]: Professional Linux User Group Security Engineering Unit 5 [Web Book](https://professionallinuxusersgroup.github.io/psc/u5ws.html) ProLUG, 2025.
[^2]: Rocky Documentation: Software Management [Web Book](https://docs.rockylinux.org/books/admin_guide/13-softwares/) Rocky Docs, 2025.
[^3]: Google Search Engine [Web](https://www.google.com) Search Engine, 2025.
[^4]: Epel Documentation [Web Docs](https://www.redhat.com/en/blog/whats-epel-and-how-do-i-use-it/) IBM, 2025.


