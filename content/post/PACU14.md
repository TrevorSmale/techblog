+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 14 🐧"
date = "2024-12-22"
description = "ProLUG Admin Course Unit 14"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems", "Declaritive", "Configuration", "Ansible", "IaC"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Ansible Automation

Ansible is an open-source automation tool used for configuration management, application deployment, and IT orchestration, enabling tasks to be executed on multiple systems simultaneously without the need for agents. It uses simple YAML-based playbooks and SSH for communication, making it efficient and easy to learn for managing infrastructure.

---

## Discussion Post 1: 

Refer to your Unit 5 scan of the systems. You know that Ansible
is a tool that you want to maintain in the environment. Review this online documentation:
https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html

### Make an inventory of the servers, grouped any way you like.

[warewulf]
192.168.200.25

[ubuntu]
192.168.200.[101:103]
192.168.200.[201:203]

[rockynodes]
192.168.200.[51:69]

### What format did you choose to use for your inventory?

INI. YAML seems to be the clear choice as it allows for a more declarative inventory. However, while working in the study group it was easier to edit INI without making indentation errors.

### What other things might you include later in your inventory to make it more useful?

I can add quite a few interesting things to an inventory file to make it more useful. Once the file reaches a certain size, it is better to break it out into separate unit files that are nested for things like Hosts, Host Variables, Production, Staging etc...

Some notable things I think make things pretty powerful are:
- Dynamic Inventorying
- Vault encrypted secrets
- Jinja2 Dynamic variables

---

## Discussion Post 2:

You have been noticing drift on your server configurations, so
you want a way to generate a report on them every day to validate the configurations are the
same. 

- Use any lab in here to find ideas: https://killercoda.com/het-tanis/course/Ansible-
Labs Use this webhook to send your relevant data out to our sandbox.
https://discord.com/api/webhooks/1317659221604433951/uyKpuq8fNNNSEyCra4n33PakI
Bk-XtTn1WrwTpHs9BcgkIu7URPV_Gd5HJCRX0_EJVUT

---
- name: System Information Check
  hosts: all
  become: yes
  tasks:
    - name: Check kernel version
      command: uname -r
      register: kernel_version
    - name: Display kernel version
      debug:
        msg: "Kernel Version: {{ kernel_version.stdout }}"

    - name: Display kernel command line
      command: cat /proc/cmdline
      register: kernel_cmdline
    - name: Debug kernel command line
      debug:
        msg: "Kernel Command Line: {{ kernel_cmdline.stdout }}"

    - name: Check hardware information
      command: lshw
      register: hardware_info
    - name: Debug hardware information
      debug:
        msg: "Hardware Information: {{ hardware_info.stdout }}"

    - name: List installed RPM packages
      command: rpm -qa
      register: installed_rpms
    - name: Debug installed RPM packages
      debug:
        msg: "Installed RPMs: {{ installed_rpms.stdout }}"

    - name: Check active services
      command: systemctl list-units --type=service --state=running
      register: active_services
    - name: Debug active services
      debug:
        msg: "Active Services: {{ active_services.stdout }}"

    - name: List system users
      command: cat /etc/passwd
      register: system_users
    - name: Debug system users
      debug:
        msg: "System Users: {{ system_users.stdout }}"

    - name: Check last login information
      command: last
      register: last_login
    - name: Debug last login information
      debug:
        msg: "Last Login Information: {{ last_login.stdout }}"

    - name: Display currently logged-in users
      command: w
      register: logged_in_users
    - name: Debug currently logged-in users
      debug:
        msg: "Logged-in Users: {{ logged_in_users.stdout }}"

    - name: Display ISO time
      command: date --iso-8601=seconds
      register: iso_time
    - name: Debug ISO time
      debug:
        msg: "ISO Time: {{ iso_time.stdout }}"
        
---

## Discussion Post 3:

Using ansible module for git, pull down this repo: 👍
https://github.com/het-tanis/HPC_Deploy.git

### How is the repo setup?

We have 3 playbooks in the root of the directory. These playbooks utilize roles defined in the roles subdirectory.
Playbook 01 gathers facts about nfs using roles defined in a subdirectory called roles.
Playbook 02 gathers data from a target system using roles defined in a subdirectory called data-gather.
Playbook 03 updates and installs using roles defined in a subdirectory called packages_update/tasks & packages_install/tasks.


### What is in the roles directory?

Partially answered for the previous question.
Roles are structured with dedicated directories for tasks, handlers, and templates. 

### How are these playbooks called, and how do roles differ from tasks?

These playbooks incorporate roles based on specific conditions, executing the tasks defined within each role. When a role is included, the playbook inherits all its contents.

---

## Digging Deeper

### I have a large amount of labs to get you started on your Ansible Journey (all free):
https://killercoda.com/het-tanis/course/Ansible-Labs 👍

### Find projects from our channel Ansible-Code, in Discord and find something that is
interesting to you. 👍

### Use Ansible to access secrets from Hashicorp Vault: https://killercoda.com/het-
tanis/course/Hashicorp-Labs/004-vault-read-secrets-ansible 👍

---

# Lab Work 🧪  👍

### Create an inventory:

1. While still in the /root/ansible_madness directory, create a file hosts
vi /root/ansible_madness/hosts
Populate the file as follows

[servers]

- 192.168.200.101
- 192.168.200.102
- 192.168.200.103

Run Ad Hoc commands against your servers
This will test your connection into all the servers.

### 1. ansible servers -i hosts -u inmate -k -m shell -a uptime
Use this password: LinuxR0cks1!

Do the same thing, but this time be verbose

### 2. ansible -vvv servers -i hosts -u inmate -k -m shell -a uptime
Create a playbook to push over files.

### 3. echo "This is my file <yourname>" > somefile

### 4. vi deploy.yaml

Populate it as follows:

    name: Start of push playbook

    hosts: servers
    vars:
    gather_facts: True
    become: False
    tasks:
    name: Copy somefile over at {{ ansible_date_time.iso8601_basic_short }}
    copy:
    src: /root/ansible_madness/somefile
    dest: /tmp/somefile.txt

### 5. Execute your playbook

ansible-playbook -i hosts -k deploy.yaml

### 6. Verify that your file pushed everywhere
ansible servers -i hosts -u inmate -k -m shell -a “ls -l /tmp/somefile”
Pull down a github repo
1. git clone https://github.com/het-tanis/HPC_Deploy.git
cd HPC_Deploy
What do you see in here?
What do you need to learn about more to deploy some of these tools?
Can you execute some of these, why or why not?

---

## Reflection Questions

1. What questions do you still have about this week?

2. How can you apply this now in your current role in IT? If you’re not in IT, how can you
look to put something like this into your resume or portfolio?

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis

