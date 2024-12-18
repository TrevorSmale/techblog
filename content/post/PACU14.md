+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 14 🐧"
date = "2024-12-17"
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

### What format did you choose to use for your inventory?

### What other things might you include later in your inventory to make it more useful?

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

## Discussion Post 3:

Using ansible module for git, pull down this repo:
https://github.com/het-tanis/HPC_Deploy.git

### How is the repo setup?

### What is in the roles directory?

### How are these playbooks called, and how do roles differ from tasks?

---

## Digging Deeper

### I have a large amount of labs to get you started on your Ansible Journey (all free):
https://killercoda.com/het-tanis/course/Ansible-Labs

### Find projects from our channel Ansible-Code, in Discord and find something that is
interesting to you.

### Use Ansible to access secrets from Hashicorp Vault: https://killercoda.com/het-
tanis/course/Hashicorp-Labs/004-vault-read-secrets-ansible

---

# Lab Work 🧪

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

