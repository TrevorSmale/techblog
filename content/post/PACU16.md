+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 16 🐧"
date = "2024-12-28"
description = "ProLUG Admin Course Unit 16"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems", "Trouble Shooting", "Incident Response"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Incident Response

Incident response is a structured approach to identifying, managing, and resolving unexpected events such as security breaches, system failures, or misconfigurations. It aims to minimize disruption, mitigate damage, and restore normal operations while implementing lessons learned to prevent future incidents.

Responding to incidents is a stressful event because it can involve many stakeholders and little time. This week we exercised our skills by live debugging in front of our peers on a remote host. The problems all related to failure modes and misconfiguration and the exercise was rewarding in that I learned a lot as always, and built some confidence.

---

# Incident Response / Troubleshooting Scenarios 🧑‍💻

## Scenario #1: Web Server Not Running 🕸️

**Objective**: Ensure the web server is running and responding on port 80.  
**Steps**:
- **Verify web server service**:
  - Run: `systemctl enable --now httpd` (or similar command).
- **Check open ports**:
  - Run: `ss -ntulp`.
  - Identify if the server is running on port 8087 instead of 80.
  - Edit the configuration:
    - File: `/etc/httpd/conf/httpd.conf`.
    - Change `Listen 8087` to `Listen 80`.
  - Restart the service: `systemctl restart httpd`.
- **Ensure external connectivity**:
  - Check the firewall status: `systemctl status firewalld`.
  - Options:
    - Disable the firewall: `systemctl stop firewalld`.
    - Open port 80 if needed.
- **Final step**:
  - REBOOT the lab machine.

---

## Scenario #2: Mount Point /space Not Working 💾

**Objective**: Set up a 9GB partition on the `/space` mount point using LVM.  
**Steps**:
1. **Verify `/space` setup**:
   - Confirm the partition is not properly set up.
2. **Create LVM setup**:
   - Identify disks: `fdisk -l | grep -i xvd`.
   - Create physical volumes: `pvcreate /dev/xvd<disk>`.
   - Create a volume group:
     - Run: `vgcreate space /dev/xvd<disk1> /dev/xvd<disk2> /dev/xvd<disk3>`.
   - Create a logical volume:
     - Run: `lvcreate -n space -l +100%FREE space`.
3. **Format the logical volume**:
   - Create a filesystem: `mkfs.ext4 /dev/mapper/<logical_volume_name>`.
4. **Mount the filesystem**:
   - Create the mount point: `mkdir /space`.
   - Add an entry in `/etc/fstab`:
     ```fstab
     /dev/mapper/<logical_volume_name> /space <ext4 or xfs> defaults 1 2
     ```
   - Mount the filesystem: `mount -a`.
5. **Final step**:
   - REBOOT the lab machine.

---

## Scenario #3: System Not Updating 📦

**Objective**: Fix the system to allow updates via `dnf` and ensure kernel updates.  
**Steps**:
1. **Fix DNF repository configuration**:
   - Edit `/etc/yum.repos.d/rocky.repo`:
     - Set `enabled=1` for all necessary repositories.
   - Check `/etc/yum.repos.d/rocky.repo.orig` for reference.
   - Fix the EPEL repository the same way.
2. **Verify kernel updates**:
   - Edit `/etc/yum.conf`:
     - Comment out the line: `exclude=kernel*`.
3. **Final step**:
   - REBOOT the lab machine if necessary.

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

