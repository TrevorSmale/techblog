+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 6 🐧"
date = "2024-10-21"
description = "ProLUG Admin Course Unit 6"
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

# Security – Firewalld & UFW 🔥🧱

In this unit, we explore essential concepts in firewall management using Firewalld and UFW. A firewall acts as a security system, controlling the flow of traffic between networks by enforcing rules based on zones —logical areas with different security policies. Services are predefined sets of ports or protocols that firewalls allow or block, and zones like DMZ (Demilitarized Zone) provide added security layers by isolating public-facing systems. Stateful packet filtering tracks the state of connections, allowing more dynamic rules, while stateless packet filtering inspects individual packets without connection context. Proxies facilitate indirect network connections for security and privacy, while advanced security measures such as Web Application Firewalls (WAF) and Next-Generation Firewalls (NGFW) offer specialized protection against modern threats.

---

# Lab Work 🧪🥼

## Intro

This week, we dove deep into configuring and testing firewall settings in our Discord study group. I had several virtual machines set up in my ProxMox home lab, and we experimented while completing the lab work. As usual, we went on several tangents, verifying ideas. In total, we spent over 5 hours running commands, experimenting with different configurations, breaking things, and debating solutions. By the end of the session, I gained a practical understanding of Firewalld configuration, packet sending, and packet tracing with Wireshark. It was frustrating at times, but ultimately rewarding.

### Sending & Receiving test packets experiment

#### 1. So we set up one server to receive a packet:

        nc -l -p -u 12345 > received_file

#### 2. Sending from another server and noted that the packet was recorded in **received_file**

        echo 'message1' | nc -u server_b_ip 12345

#### 3. We set Firewalld to block udp port 12345

        sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.2.166" port protocol="udp" port="12345" accept' --permanent

#### 4. So we set up one server to receive a packet:

        nc -l -p -u 12345 > received_file

#### 5. Sending from another server:

        echo 'Blocked' | nc -u server_b_ip 12345

#### 6. We then noted that this packet did not come through

#### 7. We then set up netcat to listen for packets on udp port 12346 'slightly different'

        nc -l -p -u 12346 > received_file

#### 8. Sending from another server and noted that the packet was recorded in **received_file**

        echo 'message2' | nc -u server_b_ip 12346

#### 9. Then we busted open WireShark on the packet receiving VM and turned on general packet scanning

        echo 'mom' | nc -u server_b_ip 12346 

#### 10. We then looked at all of the ways we could inspect the packets including additional data bits over UDP for no apparent reason.

Fin

---

# Types of Firewalls 🔍

## Firewalld 🔥🧱

Uses **zones** to define the level of trust for network connections, making it easy to apply different security settings to various types of connections (like home, public, or work). It’s dynamic, meaning changes can be made without restarting the firewall, ensuring smooth operation.

### Zones 🍱

The concept is specific to Firewalld. Zones are a predefined set of firewall rules that determine the level of trust assigned to a network connection. Zones allow you to apply different security policies to different network interfaces based on how much you trust the network.

### Common Commands

        firewall-cmd --state
Checks if Firewalld is running.

        firewall-cmd --get-active-zones
Lists all active zones and the interfaces associated with them.

        firewall-cmd --get-default-zone
Displays the default zone for new interfaces or connections.

        firewall-cmd --set-default-zone=ZONE
Changes the default zone to the specified zone.

        firewall-cmd --zone=ZONE --add-service=SERVICE
Allows a service (e.g., SSH, HTTP) in the specified zone.

        firewall-cmd --zone=ZONE --remove-service=SERVICE
Removes a service from the specified zone.

        firewall-cmd --zone=ZONE --add-port=PORT/PROTOCOL
Opens a specific port (e.g., 80/tcp) in the specified zone.

        firewall-cmd --zone=ZONE --remove-port=PORT/PROTOCOL
Closes a specific port in the specified zone.

        firewall-cmd --reload
Reloads the Firewalld configuration without dropping active connections.

        firewall-cmd --list-all
Lists all the rules and settings in the active zone.

        firewall-cmd --permanent
Applies changes permanently (used with other commands to ensure changes persist after reboots).

        firewall-cmd --runtime-to-permanent
Converts the current runtime configuration to a permanent one.

### Zone example

- A public zone might have stricter rules, blocking most traffic except for essential services like web browsing.
- A home zone could allow more open traffic, such as file sharing, because the network is more trusted.

## UFW 🔥🧱

**Uncomplicated Firewall** is a user-friendly firewall designed to simplify the process of controlling network traffic by allowing or blocking connections. UFW is commonly used on **Ubuntu** and provides easy commands for setting up firewall rules, making it ideal for beginners. Despite it is simplicity, it is powerful enough to handle complex configurations.

### Default Deny Policy 🔐
- By default, UFW denies all incoming connections while allowing outgoing ones. This enhances security by requiring users to explicitly allow any incoming traffic.

### Common Commands

        sudo ufw status
Displays the current status of UFW and active rules.

        sudo ufw enable
Enables the UFW firewall.

        sudo ufw disable
Disables the UFW firewall.

        sudo ufw default deny incoming
Sets the default policy to deny all incoming connections.

        sudo ufw default allow outgoing
Sets the default policy to allow all outgoing connections.

        sudo ufw allow PORT
Allows traffic on a specific port (e.g., `sudo ufw allow 22` to allow SSH).

        sudo ufw deny PORT
Denies traffic on a specific port.

        sudo ufw delete allow PORT
Removes a previously allowed rule for a port.

        sudo ufw allow SERVICE
Allows traffic for a service by name (e.g., `sudo ufw allow ssh`).

        sudo ufw deny SERVICE
Denies traffic for a service by name.

        sudo ufw allow from IP
Allows traffic from a specific IP address.

        sudo ufw deny from IP
Denies traffic from a specific IP address.

        sudo ufw allow proto PROTOCOL from IP to any port PORT
Allows traffic for a specific protocol, source IP, and port (e.g., `sudo ufw allow proto tcp from 192.168.1.100 to any port 80`).

        sudo ufw reset
Resets all UFW rules to default.

        sudo ufw reload
Reloads UFW rules without disabling the firewall.

## WAF 🔥🧱

**Web Application Firewall** is a security system designed to protect web applications by filtering and monitoring HTTP traffic between a web application and the internet. It helps prevent common web-based attacks like SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF) by analyzing the incoming and outgoing traffic and blocking malicious requests. Unlike traditional firewalls that focus on network security, a WAF specifically targets the security of web applications and can be an important part of a layered defense strategy.

### More Sophisticated 🍷

are generally more sophisticated than Firewalld or UFW because they operate at the application layer (Layer 7) of the OSI model. Blocking traffic is one thing, but packet inspection is another.

### Quite a few ⚖️
There are many Web Application Firewalls out there that cover specific cloud platforms or web services. Here is a list of some popular ones:
- AWS WAF
- Cloudflare WAF
- F5 Advanced WAF
- Imperva WAF
- ModSecurity
- Barracuda WAF
- Sucuri WAF
- Akamai Kona Site Defender
- Fortinet FortiWeb

## NGFW 🔥🧱🧠

**Next-Generation Firewall** is an advanced type of firewall that goes beyond traditional firewall features like packet filtering. It combines standard firewall capabilities with more advanced functionalities such as deep packet inspection (DPI), intrusion prevention systems (IPS), and application-level control. NGWs can inspect and control traffic at a more granular level, allowing administrators to set security rules based on specific applications, users, or behaviors.

### Features of a typical NGFW

- **Deep Packet Inspection (DPI)**: Examines the content of data packets, not just their headers, allowing the firewall to identify and block threats hidden in the traffic.
- **Intrusion Detection and Prevention System (IDS/IPS)**: Monitors network traffic for suspicious activity and can take action (like blocking or alerting) to prevent attacks in real-time.
- **Application Awareness and Control**: Recognizes and manages specific applications (e.g., Facebook, Skype) regardless of port or protocol, allowing for fine-grained traffic control.
- **Advanced Malware Protection (AMP)**: Detects and blocks malware using both signature-based detection and behavioral analysis to prevent malware from entering the network.
- **SSL/TLS Decryption**: Decrypts encrypted traffic (HTTPS) for inspection to detect threats hiding inside secure channels.
- **User Identity Integration**: Applies firewall rules based on user identity or group membership rather than just IP addresses, providing more flexible access control.
- **Threat Intelligence Feeds**: Uses real-time threat data from global databases to protect against emerging threats and malicious IP addresses or domains.
- **Cloud-Delivered Security**: Provides scalable and flexible cloud-based protection services such as sandboxing, traffic analysis, and updates for zero-day attacks.
- **Virtual Private Network (VPN) Support**: Allows secure, encrypted connections for remote users or between different networks (site-to-site or remote access VPNs).
- **URL Filtering**: Controls access to websites based on categories (e.g., social media, gambling) or specific URLs, helping enforce acceptable use policies.
- **Quality of Service (QoS)**: Prioritizes certain types of traffic, ensuring that critical applications receive the necessary bandwidth and reducing congestion.
- **Zero-Trust Network Segmentation**: Implements policies based on strict access control, ensuring that users and devices only access the resources they are explicitly permitted.
- **Sandboxing**: Isolates suspicious files or code in a secure environment to detect malicious behavior without affecting the rest of the network.
- **Logging and Reporting**: Provides detailed logs and reports on network traffic, blocked threats, and firewall activity for auditing and troubleshooting.

### NGFW Products

- Palo Alto Networks NGFW
- Cisco Firepower NGFW
- Fortinet FortiGate NGFW
- Check Point NGFW
- Sophos XG NGFW
- Juniper Networks SRX Series NGFW
- Barracuda CloudGen NGFW
- SonicWall NGFW
- WatchGuard Firebox NGFW
- Forcepoint NGFW
- PfSense NGFW

### Experience with PFSense

I am familiar with PfSense as it is **Open Source** and popular among the Homelab enthusiasts because it is offers expansive features built upon FreeBSD which has killer networking.

### Some limitations

#### Pre-packaged Features
Commercial NGFWs (e.g., Palo Alto Networks, Cisco Firepower) often come with built-in advanced features such as cloud-delivered threat intelligence, AI-powered threat detection, and sandboxing for zero-day threats. While pfSense can be extended with third-party packages, it doesn’t natively offer the same level of seamless integration or automation.

#### Unified Management
Commercial NGFWs typically provide a centralized management console for handling multiple firewalls across large networks. While pfSense can handle multiple installations, managing them requires more manual effort and may not be as streamlined as the enterprise-grade management consoles of commercial NGFWs.

#### Enterprise Support
pfSense relies on community and third-party support, whereas commercial NGFWs offer direct vendor support with service level agreements (SLAs), which can be crucial for large enterprises needing guaranteed response times and assistance.

#### Threat Intelligence
NGFWs like those from Palo Alto or Cisco often integrate with real-time global threat intelligence networks, offering constant updates about emerging threats. While pfSense can be configured with tools like Snort for intrusion detection, it lacks the built-in, cloud-powered intelligence found in commercial NGFWs.

---

# Case Study #1 🤔

A ticket has come in from an application team. Some of the servers your team built for them last week have not been reporting up to enterprise monitoring and they need it to be able to troubleshoot a current issue, but they have no data. You jump on the new servers and find that your engineer built everything correctly and the agents for node_exporter, ceph_exporter and logstash exporter that your teams use. But, they also have adhered to the new company standard of firewalld must be running. No one has documented the ports that need to be open, so you’re stuck between the new standards and fixing this problem on live systems.

### 1. Initial Research

Findings:
- node_exporter typically uses port **9100**
- ceph_exporter may use port **9128**
- logstash commonly uses ports **5044**

### 2. Checking Configs

#### A. LogStash Config

        cat /etc/logstash/conf.d/

#### B. node exporter config

        cat /etc/systemd/system/node_exporter.service

#### C. ceph exporter config

        cat /etc/systemd/system/ceph_exporter.service

# Or

### 3. Gathering Socket Statistics

        sudo ss -tuln | grep LISTEN

#### Options Breakdown

-t: Show TCP sockets.
-u: Show UDP sockets.
-l: Show listening sockets, i.e., those waiting for incoming connections.
-n: Show the output numerically, without resolving service names (e.g., display IP addresses and port numbers instead of domain names or service names like “http”).

### Q: 1. As you’re looking this up, what terms and concepts are new to you?

Basically all of the concepts used are new to me. I am not very well versed in networking, network scanning or inspecting service configs.
So this became a research and practice exercise that has shown me quite a lot of new tricks.

### Q: 2. What are the ports that you need to expose? How did you find the answer?

Theoretically I would expose port **9100**, **9128**, **5044** from research. Furthermore, I now know how to check configs and/or gathering sockets statistics

### Q: 3. What are you going to do to fix this on your firewall?

I would add these services to the internal zone:

        firewall-cmd --zone=ZONE --add-service=SERVICE

Allows a service (e.g., SSH, HTTP) in the specified zone.

---

# Case Study #2 🤔

A manager heard you were the one that saved the new application by fixing the firewall. They get your manager to approach you with a request to review some documentation from a vendor that is pushing them hard to run a WAF in front of their web application. You are “the firewall” guy now, and they’re asking you to give them a review of the differences between the firewalls you set up (which they think should be enough to protect them) and what a WAF is doing.

### Q: 1. What do you know about the differences now?

#### Traditional Firewalls (firewalld/ufw):
- Operate at network and transport layers (OSI Layer 3 & 4).
- Control traffic based on IP addresses, ports, and protocols.
- Block or allow entire network connections.

#### Web Application Firewalls (WAFs):
- Operate at the application layer (OSI Layer 7).
- Inspect HTTP/HTTPS traffic, focusing on web application security.
- Protect against attacks like SQL injection, XSS, and other web vulnerabilities.

### Q: 2. What are you going to do to figure out more?
- Dedicate time to researching effective WAF solutions.
- Identifying suitable solutions at 3 budget scales.
- Try to understand the additional labour behind management additional tools.

### Q: 3. Prepare a report for them comparing it to the firewall you did in the first discussion.

# Report 🗒️

### Evaluation of Implementing a Web Application Firewall **WAF**

### **Prepared by:** Treasure 

### **Date:** Oct 20th 2024  

### **Subject:** Evaluation of WAF Implementation Suitability and Comparison with Traditional Firewalls

---

## **1. Introduction**

This report has been prepared in response to a request to evaluate the suitability of implementing a Web Application Firewall (WAF) within our infrastructure. The aim of this report is to:
- Compare WAF technology with traditional firewall solutions currently implemented.
- Assess the benefits and limitations of each.
- Provide recommendations based on the findings.

## **2. Objectives**

The key objectives of this evaluation are:
- To determine the suitability of WAF in enhancing our web application security.
- To identify potential risks and benefits associated with the deployment of WAF.
- To compare traditional firewall solutions with WAF in terms of functionality, security, and cost.
- To make recommendations based on the current and future needs of our IT infrastructure.

## **3. Comparison of Technologies**

### **3.1 Traditional Firewalls (firewalld/ufw)**
- **Primary Function**: Control and filter network traffic based on IP addresses, ports, and protocols.
- **Strengths**: 
  - Blocks unwanted connections at the network level.
  - Suitable for general network protection.
  - Easy to configure and manage.
- **Limitations**: 
  - Does not inspect web traffic at the application level.
  - Cannot protect against specific web application attacks (e.g., SQL injection, XSS).

### **3.2 Web Application Firewalls (WAF)**
- **Primary Function**: Protect web applications by filtering and monitoring HTTP/HTTPS traffic.
- **Strengths**: 
  - Protects against common web application vulnerabilities (e.g., SQL injection, XSS).
  - Monitors web traffic to block malicious requests.
  - Can provide real-time threat detection and logging.
- **Limitations**: 
  - May require more resources and specialized configuration.
  - Focused solely on web applications, not general network traffic.

### **3.3 Key Differences**

| Feature | Traditional Firewall | Web Application Firewall (WAF) |
|---------|----------------------|-------------------------------|
| **Layer** | Network (Layer 3/4) | Application (Layer 7) |
| **Traffic Type** | IP, ports, protocols | HTTP/HTTPS, web requests |
| **Use Case** | General network security | Web application protection |
| **Threat Coverage** | Blocks IP-based threats | Mitigates web vulnerabilities (SQLi, XSS) |
| **Cost** | Typically lower | Generally higher due to specialized focus |

## **4. Key Considerations for WAF Implementation**

### **4.1 Security Benefits**
- Enhanced protection against web-specific attacks.
- Ability to monitor and block suspicious activity in real-time.
- Added layer of security on top of traditional network firewalls.

### **4.2 Cost Analysis**
- **Initial Investment**: The upfront cost of acquiring and configuring a WAF solution.
- **Ongoing Costs**: Maintenance, updates, and potential personnel training.

### **4.3 Operational Impact**
- May require additional resources for setup, monitoring, and incident response.
- Potential need for collaboration between the security and development teams to ensure smooth integration.

## **5. Risk Assessment**

- **Without WAF**: Increased vulnerability to web application-specific threats, such as cross-site scripting (XSS) and SQL injection, especially for critical applications.
- **With WAF**: Increased security for web applications but requires ongoing monitoring and adjustment to ensure performance and efficacy.

## **6. Recommendations**

Based on the evaluation, I recommend the following:
1. **Implement a WAF**: Due to the increasing reliance on web applications and the rise in web-based attacks, implementing a WAF would provide an essential layer of security.
2. **Maintain Traditional Firewalls**: Existing firewalls should continue to be used for network-level protection alongside the WAF.
3. **Pilot Implementation**: Begin with a limited deployment of WAF for high-risk applications to evaluate performance and cost before a full-scale rollout.
4. **Staff Training**: Ensure the security and IT teams are trained in WAF management to maximize its effectiveness.

## **7. Conclusion**

The implementation of a Web Application Firewall is a strategic move to protect our web applications from evolving security threats. While traditional firewalls remain crucial for network security, they cannot defend against the types of attacks WAFs are designed to mitigate. By implementing both WAF and traditional firewall solutions, we can ensure comprehensive security coverage across both network and application layers.

## **8. Next Steps**

- Further evaluation of potential WAF solutions based on budget, compatibility, and scalability.
- Engage with the security team for a detailed implementation plan.
- Prepare a pilot program for critical applications and monitor its performance.

### **Approved by:** Bob Saggit 

### **Date:** October 25th 2024

---

### Definitions

- **Firewall**: A security device that monitors and controls incoming and outgoing network traffic based on predetermined security rules.
- **Zone**: A defined area within a network that contains systems with similar security requirements, separated by a firewall.
- **Service**: A specific type of network functionality, like HTTP or DNS, that can be allowed or blocked by a firewall.
- **DMZ**: A "Demilitarized Zone" is a network segment that serves as a buffer between a secure internal network and untrusted external networks.
- **Proxy**: A server that acts as an intermediary for requests between clients and servers, often used for filtering, security, or caching.
- **Stateful packet filtering**: A firewall feature that tracks the state of active connections and makes filtering decisions based on the connection's state.
- **Stateless packet filtering**: A type of firewall filtering that analyzes each packet independently without considering the state of the connection.
- **WAF**: A Web Application Firewall that protects web applications by filtering and monitoring HTTP/HTTPS traffic for threats like SQL injection and XSS.
- **NGFW**: A Next-Generation Firewall that combines traditional firewall functions with additional features like application awareness, integrated intrusion prevention, and advanced threat detection.

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis


[^1]: MITRE | ATT&CK [Site](https://attack.mitre.org/) Knowledge Base.