+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 11 🐧"
date = "2024-11-28"
description = "ProLUG Admin Course Unit 11"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems", "Monitoring"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Monitoring Systems

In this unit, we explore monitoring systems, which often consist of multiple interconnected components. At its core, monitoring involves carefully exposing system data and transmitting it to tools for analysis and alerting. From my experience with Prometheus and Grafana—two widely used and versatile solutions—I’ve seen how effective these tools can be for various scenarios. However, many other tools are also available. One of my key takeaways from the unit’s readings and labs was the importance of careful data exposure. Much like setting permissions in a Linux system, it’s crucial to determine what data can be accessed and who is allowed to see it in the reporting chain. System information, if mishandled, can easily become a double-edged sword.

---

## Discussion Post 1 

### Scenario

You’ve heard the term “loose coupling” thrown around the
office about a new monitoring solution coming down the pike. You find a good resource and
read the section on “Prefer Loose Coupling” https://sre.google/workbook/monitoring/

#### 1. What does “loose coupling” mean, if you had to summarize to your junior team
members?

Loose coupling means the components of a system can operate independently, yet still work together when combined. This design allows individual components to be swapped or replaced with minimal disruption to the overall system. In contrast, a strongly coupled system binds its components so tightly that altering one would disrupt or even break the entire system’s functionality.

#### 2. What is the advantage given for why you might want to implement this type of tooling in your monitoring? Do you agree? Why or why not?

The advantage of a loosely coupled monitoring system lies in its flexibility to evolve over time. Systems change, requirements shift, and new tools emerge, making adaptability essential. A design that allows components to be replaced or upgraded with minimal disruption is highly valuable—not only for an organization aiming to maintain efficiency but also for the administrators and engineers responsible for ensuring stability and resolving issues.

#### 3. They mention “exposing metrics” what does it mean to expose metrics? What happens to metrics that are exposed but never collected?

Exposing metrics involves making system information accessible for monitoring and analysis. However, this must be approached with caution, as exposing such information can introduce vulnerabilities. Simply exposing data without actively collecting or utilizing it needlessly increases security risks without providing any benefit.

---

## Discussion Post 2

### Scenario

Your HPC team is asking for more information about how CPU 0
is behaving on a set of servers. Your team has node exporter writing data out to Prometheus
(Use this to simulate https://promlabs.com/promql-cheat-sheet/).

#### 1. Can you see the usage of CPU0 and what is the query?

Yes one can use a query that focuses on the metrics provided by the Node Exporter. Specifically, filter for CPU 0 and its usage. 

    100 - (avg by (instance) (irate(node_cpu_seconds_total{cpu="0", mode="idle"}

#### 2. Can you see the usage of CPU0 for just the last 5 minutes and what is the query?

Yes

    100 - (avg by (instance) (rate(node_cpu_seconds_total{cpu="0", mode="idle"}[5m])) * 100)

#### 3. You know that CPU0 is excluded from Slurm, can you exclude that and only pull the user and system for the remaining CPUs and what is that query?

Yes

    sum by (instance) (rate(node_cpu_seconds_total{cpu!="0", mode=~"user|system"}[5m])) * 100

---

## Digging Deeper

#### 1. Read the rest of the chapter https://sre.google/workbook/monitoring/ and note anything else of interest when it comes to monitoring and dashboarding.
#### 2. Look up the “ProLUG Prometheus Certified Associate Prep 2024” in Resources -> Presentations in our ProLUG Discord. Study that for a deep dive into Prometheus.
#### 3. Complete the project section of “Monitoring Deep Dive Project Guide” from the prolug-projects section of the Discord. We have a Youtube video on that project as well. https://www.youtube.com/watch?v=54VgGHr99Qg

## Labs

https://killercoda.com/het-tanis/course/Linux-Labs/102-monitoring-linux-logs

https://killercoda.com/het-tanis/course/Linux-Labs/103-monitoring-linux-telemetry

https://killercoda.com/het-tanis/course/Linux-Labs/104-monitoring-linux-Influx-Grafana

2. While completing each lab think about the following:

a. How does it tie into the diagram below?

b. What could you improve, or what would you change based on your previous administration
experience.

---

### 1. Install Grafana

#### 1.1 Create a New Repository Configuration

```bash
sudo vim /etc/yum.repos.d/grafana.repo
```

Paste the following:

```ini
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
```

#### 1.2 Verify the Repository

```bash
sudo dnf repolist
sudo dnf clean all
```

Expected Output:

```
repo id                                      repo name
appstream                                    Rocky Linux 8 - AppStream
baseos                                       Rocky Linux 8 - BaseOS
extras                                       Rocky Linux 8 - Extras
grafana                                      grafana/spl
```

#### 1.3 Check the Grafana Package

```bash
sudo dnf info grafana
```

Example Output:

```
Importing GPG key 0x24098CB6:
Userid     : "Grafana"
Fingerprint: 4E40 DDF6 D76E 284A 4A67 80E4 8C8C 34C5 2409 8CB6
From       : https://packages.grafana.com/gpg.key
Is this ok [y/N]: y
```

#### 1.4 Install Grafana

```bash
sudo dnf install grafana -y
```

#### 1.5 Enable and Start Grafana Service

```bash
sudo systemctl enable --now grafana-server
sudo systemctl status grafana-server
```

#### 1.6 Configure Firewall

```bash
sudo firewall-cmd --add-service=grafana --permanent
sudo firewall-cmd --add-port=3000/tcp --permanent
sudo firewall-cmd --reload
```

#### 1.7 Create and Edit Configuration File

```bash
sudo vim /etc/grafana/grafana.ini
```

Update the following settings:

```ini
[server]
http_port = 4000
domain = grafana.example.io

[analytics]
reporting_enabled = false
```

Restart the service:

```bash
sudo systemctl restart grafana-server
```

---

### 2. Reverse Proxy Setup with NGINX

#### 2.1 Install NGINX

```bash
sudo dnf install nginx -y
```

#### 2.2 Create a Server Block for Grafana

```bash
sudo vim /etc/nginx/conf.d/grafana.conf
```

Paste the configuration:

```nginx
map $http_upgrade $connection_upgrade {
  default upgrade;
  '' close;
}

server {
  listen 80;
  server_name grafana.example.io;
  rewrite ^ https://$server_name$request_uri? permanent;
}

server {
  listen 443 ssl http2;
  server_name grafana.example.io;

  ssl_certificate /etc/letsencrypt/live/grafana.example.io/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/grafana.example.io/privkey.pem;

  location / {
    proxy_pass http://localhost:3000/;
  }

  location /api/live {
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_pass http://localhost:3000/;
  }
}
```

#### 2.3 Verify NGINX Configuration

```bash
sudo nginx -t
```

Expected Output:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

#### 2.4 Start and Enable NGINX

```bash
sudo systemctl enable --now nginx
sudo systemctl status nginx
```

---

### 3. Install Prometheus

#### 3.1 Add a New User and Directories

```bash
sudo adduser -M -r -s /sbin/nologin prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

#### 3.2 Download and Extract Prometheus

```bash
cd /usr/src
wget https://github.com/prometheus/prometheus/releases/download/v3.0.1/prometheus-3.0.1.linux-amd64.tar.gz
tar -xzf prometheus-3.0.1.linux-amd64.tar.gz
cd prometheus-3.0.1.linux-amd64
```

#### 3.3 Move Files to Appropriate Locations

```bash
sudo mv prometheus /usr/local/bin/
sudo mv promtool /usr/local/bin/
sudo mv consoles /etc/prometheus
sudo mv console_libraries /etc/prometheus
sudo mv prometheus.yml /etc/prometheus/
```

#### 3.4 Change Ownership

```bash
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
```

#### 3.5 Configure Prometheus

```bash
sudo vim /etc/prometheus/prometheus.yml
```

Example scrape configuration:

```yaml
scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["192.168.1.10:9090"]
```

#### 3.6 Create a Systemd Service

```bash
sudo vim /etc/systemd/system/prometheus.service
```

Paste the following:

```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus     --config.file /etc/prometheus/prometheus.yml     --storage.tsdb.path /var/lib/prometheus/     --web.console.templates=/etc/prometheus/consoles     --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

#### 3.7 Start Prometheus Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now prometheus
sudo systemctl status prometheus
```

#### 3.8 Access Prometheus

Visit `http://192.168.1.10:9090` in your browser to view the Prometheus dashboard.


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

