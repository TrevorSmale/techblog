+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 12 🐧"
date = "2024-11-30"
description = "ProLUG Admin Course Unit 12"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "Operating Systems", "Baselining", "Benchmarking"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Baselining & Benchmarking

The purpose of a baseline is not to find fault, load, or to take corrective action. A baseline simply
determines what is. You must know what is so that you can test against that when you make a change to
be able to objectively say there was or wasn't an improvement. You must know where you are at to be
able to properly plan where you are going. A poor baseline assessment, because of inflated numbers or
inaccurate testing, does a disservice to the rest of your project. You must accurately draw the first line
and understand your system's performance.

---

## Discussion Post 1: 

Your manager has come to you with another emergency. He has a meeting next week to discuss capacity planning and usage of the system with IT upper management. He doesn’t want to lose his budget, but he has to prove that the system utilization warrants spending more.

- What information can you show your manager from your systems?

You could present your manager with a progressive trend graph showing time on the x-axis and several fields on the y-axis that represent changes from a baseline, assuming the necessary data has been collected. With this information, it would be possible to predict when various system resources will reach their maximum capacity.

- What type of data would prove system utilization? (Remember the big 4: compute, memory, disk, networking)

  CPU load, process execution time, throughput. 
  Disk Operations (IOPS).
  Networking Requests and Bandwidth. 
  RAM utilization, memory paging/swapping rates.

- What would your report look like to your manager?

# Capacity Planning Report

Current and projected system utilization. By examining trends over time, we can predict when critical resources will reach their limits if no additional capacity is provisioned.

## Key Areas of Focus

1. **Compute Usage**
2. **Memory Load**
3. **Disk Resources**
4. **Networking Metrics**

## Historical Data and Trends
Below is a sample progressive trend graph over the last 6 months. The x-axis represents time (in weeks), while the y-axis shows percentage utilization relative to an established baseline.

**Example Metrics (relative to baseline):**  
- **CPU Utilization (% of baseline)**  
- **Memory Load (% of baseline)**  
- **Disk IOPS (% of baseline)**  
- **Network Throughput (% of baseline)**

    Time (Weeks):    1   2   3   4   5   6   …   20   21   22
    CPU Util(%):    50  52  55  57  60  62   …   80   82   85
    Memory(%):      45  47  50  50  52  55   …   70   73   75
    Disk IOPS(%):   30  32  35  36  38  40   …   60   63   68
    Network(%):     40  42  45  47  49  51   …   75   78   80

As time progresses, each of the key metrics is trending upward, indicating increasing load and approaching capacity thresholds.

## Projections

We can estimate the “time to ceiling” for critical resources. For instance, if CPU load is rising at an average rate of 2–3% per month, and we know that at 90% utilization the system will experience performance degradation.

**Projected Time to CPU Ceiling:** 3–5 months  
**Projected Time to Memory Ceiling:** 6–8 months  
**Projected Time to Disk IOPS Ceiling:** 8–10 months  
**Projected Time to Network Bandwidth Ceiling:** 4–6 months

## Recommendations

- **Compute**: Consider adding more CPU cores or upgrading processors before reaching the predicted 90% utilization mark.
- **Memory**: Upgrade RAM or optimize applications to reduce memory footprint.
- **Disk**: Enhance disk subsystems or switch to faster storage (e.g., SSDs) to handle projected IOPS.
- **Networking**: Increase network capacity (e.g., from 1Gb to 10Gb links) or optimize network traffic.

## Conclusion

Investment in scaling resources now will prevent future performance bottlenecks, ensuring the system can continue to meet business demands effectively.

---

## Discussion Post 2:

You are in a capacity planning meeting with a few of the architects. They have decided to add 2 more agents to your Linux Sytems, Bacula Agent and an Avamar Agent . They expect these agents to run their work starting at 0400 every morning.
 
- What do these agents do? (May have to look them up)

Bacula is an open-source suite of tools designed to automate backup tasks. It’s widely regarded for its flexibility and reliability. Dell Avamar, on the other hand, is a commercial backup automation solution. Both tools handle incremental backups using custom daemons that monitor changes over time, offering greater sophistication than simple scheduling systems like Cron. Additionally, they can manage backups across diverse, heterogeneous storage environments.

- Do you think there is a good reason not to use these agents at this timeframe?

This approach is about balancing workload. If all processes start at a fixed time, they can consume valuable resources simultaneously. The best schedule depends on the environment. For example, if the environment experiences downtime—such as a traditional office setting—starting backups at 4 a.m. might be fine. However, if services run around the clock, it’s better to stagger the tasks so they use only a fraction of the available resources at any given time. This approach also reduces the impact of failures, since not all systems are involved at once.

- Is there anything else you might want to point out to these architects about these agents they are installing?

There are several factors architects should consider. However, in the context of this discussion, performance overhead is particularly relevant. They need to ensure that the chosen backup solutions won’t overburden the system’s resources and that there’s enough “breathing room” to maintain smooth operations.

---

## Discussion Post 3: 'TODO'

Your team has recently tested at proof of concept of a new storage system. The vendor has published the blazing fast speeds that are capable of being run through this storage system. You have a set of systems connected to both the old storage system and the new storage system.

- Write up a test procedure of how you may test these two systems.

I did a bit of research regarding tooling for such a task and found FIO 'Flexible Input / Output', a program written for the purpose of testing systems with various scenarios.
Rather than using BASH, I can run more comprehensive testing with more data to analyze using FIO.

### Baseline Test

     fio --filename=/dev/new_storage_lun --direct=1 --rw=read --bs=128k --size=10G --numjobs=1 --iodepth=32 --runtime=300 --time_based --name=new_storage_seq_read
     fio --filename=/dev/old_storage_lun --direct=1 --rw=read --bs=128k --size=10G --numjobs=1 --iodepth=32 --runtime=300 --time_based --name=old_storage_seq_read

### Running Mixed Workload Tests

     fio --filename=/dev/new_storage_lun --direct=1 --rw=randrw --rwmixread=70 --bs=4k --size=10G --numjobs=4 --iodepth=16 --runtime=300 --time_based --name=new_storage_mixed

### Increased Concurrency and Scale

   - Increase `numjobs` and `iodepth` in subsequent runs to measure how performance changes:
 
     fio --filename=/dev/new_storage_lun --direct=1 --rw=read --bs=128k --size=10G --numjobs=8 --iodepth=64 --runtime=300 --time_based --name=new_storage_high_concurrency

   - Run the same tests on the old storage system and record all metrics.

### Stress/Soak Tests

   - 12-hour continuous I/O test on both storage systems.

     fio --filename=/dev/new_storage_lun --direct=1 --rw=randwrite --bs=4k --size=100G --numjobs=1 --iodepth=32 --runtime=43200 --time_based --name=new_storage_soak

### AWK Line Parsing

I would then pipe the output of these commands to **AWK** to seperate out specific datapoints to append to files for full analysis.

- How are you assuring these test are objective?

By gathering multiple datasets with varying run parameters, I can reduce statistical noise and better isolate data of interest by comparing these datasets against one another.

- What is meant by the term Ceteris Paribus, in this context?

in the context of system benchmarking means that when measuring the performance of one specific aspect of the system, all other variables and conditions are kept constant. This approach ensures that any observed changes in performance can be attributed directly to the variable under test, rather than being influenced by unrelated fluctuations in the environment or system load.

---

## Definitions & Terminology

- Benchmark
- High watermark
- Scope
- Methodology
- Testing
- Control
- Experiment
- Analytics
- Descriptive
- Diagnostic
- Predictive
- Prescriptive

## Digging Deeper (optional)

1. Analyzing data may open up a new field of interest to you. Go through some of the
free lessons on Kaggle, here: https://www.kaggle.com/learn

a. What did you learn?

b. How will you apply these lessons to data and monitoring you have already
collected as a system administrator?

2. Find a blog or article that discusses the 4 types of data analytics.

a. What did you learn about past operations?
b. What did you learn about predictive operations?

3. Download Spyder IDE (Open source)

a. Find a blog post or otherwise try to evaluate some data.
b. Perform some Linear regression. My block of code (but this requires some
additional libraries to be added. I can help with that if you need it.)

import matplotlib.pyplot as plt

from sklearn.linear_model import LinearRegression

size = [[5.0], [5.5], [5.9], [6.3], [6.9], [7.5]]
price =[[165], [200], [223], [250], [278], [315]]
plt.title('Pizza Price plotted against the size')

plt.xlabel('Pizza Size in inches')

plt.ylabel('Pizza Price in cents')

plt.plot(size, price, 'k.')

plt.axis([5.0, 9.0, 99, 355])

plt.grid(True)

model = LinearRegression()

model.fit(X = size, y = price)

#plot the regression line

plt.plot(size, model.predict(size), color='r')

Reflection Questions

1. What questions do you still have about this week?

2. How can you apply this now in your current role in IT? If you’re not in IT, how can you
look to put something like this into your resume or portfolio?

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

Install Grafana 
on the Rocky Linux system by adding the Grafana repo manually.
Red = Inputs
Blue = Outputs

1. Create a new repository configuration
sudo vim /etc/yum.repos.d/grafana.repo

Paste:

[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm    
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1

2. Verify using the DNF 

sudo dnf repolist

sudo dnf clean - verifies whether files are working

Should see:
    
repo id                                      repo name
appstream                               Rocky Linux 8 - AppStream
baseos                                      Rocky Linux 8 - BaseOS
extras                                       Rocky Linux 8 - Extras
grafana                                    grafana/spl

3. Check the grafana package on the official repository

sudo dnf info grafana

Should see something similar 👀

Importing GPG key 0x24098CB6:
Userid     : "Grafana "
Fingerprint: 4E40 DDF6 D76E 284A 4A67 80E4 8C8C 34C5 2409 8CB6
From       : https://packages.grafana.com/gpg.key
Is this ok [y/N]: y

Should see 👀

  Name         : grafana
  Version      : 8.2.5
  Release      : 1
  rchitecture : x86_64
  Size         : 64 M
  Source       : grafana-8.2.5-1.src.rpm
  Repository   : grafana
  Summary      : Grafana
  URL          : https://grafana.com
  License      : "Apache 2.0"
  Description  : Grafana

4. Install Grafana
sudo dnf install grafana -y

⏳ Takes a while...

5. Restart SystemD unit
sudo systemctl enable --now grafana-server

Verify
sudo systemctl status grafana-server

5.5. Firewall (Security)
Firewall is Managed by files present in /etc/firedwalld

cd /etc/firewalld/
ls -l

cp /usr/lib/firewalld/services/ssh.xml  /etc/firewalld/services/example.xml

sudo  firewall-cmd --add-service=grafana --permanent 

sudo firewall-cmd --add-port=3000/tcp --permanent
sudo firewall-cmd --reload

6. Create config file
sudo vim /etc/grafana/grafana.ini

7. Change the default value of:

The option 'http_addr' to 'localhost', the 'http_port' to '3000', and the 'domain' option to your domain name as below. For this example, the domain name is 'grafana.example.io'.

For non-standard port, be sure to uncomment ;
[server] 👍
http_port = 4000 👍

The public facing domain name used to access grafana from a browser
domain = grafana.example.io

7.1 Turn off the nasty default report of analytics 👺
[analytics] 
reporting_enabled = false

7.2. Restart the grafana service to apply a new configuration.

sudo systemctl restart grafana-server

Reverse Proxy Setup

1. Install NGINX

sudo dnf install nginx -y
  
2.  Create a new server block for grafana

/etc/nginx/conf.d/grafana.conf

Required to proxy Grafana Live WebSocket connections

map $http_upgrade $connection_upgrade {
  default upgrade;
  '' close;
  }
  server {
  listen      80;
  server_name grafana.example.io;
  rewrite     ^   https://$server_name$request_uri? permanent;
  }
  server {
  listen      443 ssl http2;
  server_name grafana.example.io;
  root /usr/share/nginx/html;
  index index.html index.htm;
  ssl_certificate /etc/letsencrypt/live/grafana.example.io/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/grafana.example.io/privkey.pem;
  access_log /var/log/nginx/grafana-access.log;
  error_log /var/log/nginx/grafana-error.log;
  location / {
  proxy_pass http://localhost:3000/;
  }
  
Proxy Grafana Live WebSocket connections
  location /api/live {
  rewrite  ^/(.*)  /$1 break;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection $connection_upgrade;
  proxy_set_header Host $http_host;
  proxy_pass http://localhost:3000/;
  }
  }

3. Next, verify the Nginx configuration

sudo nginx -t

Should see 👀

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful 👍

4. Start and enable the Nginx service
sudo systemctl enable --now nginx
sudo systemctl status nginx

Install Prometheus (Saturday)
## Rocky Prometheus Install

1. Add New User and Directory 'prometheus'

create a new configuration directory and data directory for the Prometheus installation.

sudo adduser -M -r -s /sbin/nologin prometheus

2. create a new configuration 
directory '/etc/prometheus' and the data directory '/var/lib/prometheus' 

(Only needed for running as service)

sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus

Note: 
    All Prometheus configuration at the '/etc/prometheus' directory, and all Prometheus data will automatically be saved to the directory '/var/lib/prometheus'.
Installing Prometheus on Rocky Linux

Install Prometheus monitoring system manually from the tarball or tar.gz file.

1. Change the working directory 
to '/usr/src' and download the Prometheus binary

cd /usr/src
wget https://github.com/prometheus/prometheus/releases/download/v3.0.1/prometheus-3.0.1.linux-amd64.tar.gz

Extract

tar -xzf ***.tar.gz

cd into folder

Run bin:
    
    ./bin 👍
    
    If bin works, proceed

2. Copy all Prometheus configurations 
to the directory '/etc/prometheus' and the binary file 'prometheus' to the '/usr/local/bin' directory.

- Move prometheus configuration 'prometheus.yml' to the directory '/etc/prometheus.

sudo mv $PROM_SRC/prometheus.yml /etc/prometheus/

- Move the binary file 'prometheus' and 'promtool' to the directory '/usr/local/bin/'.

sudo mv $PROM_SRC/prometheus /usr/local/bin/
sudo mv $PROM_SRC/promtool /usr/local/bin/

- Move Prometheus console templates and libraries to the '/etc/prometheus' directory.

sudo mv -r $PROM_SRC/consoles /etc/prometheus
sudo mv -r $PROM_SRC/console_libraries /etc/prometheus

3. Edit Prometheus configuration '/etc/prometheus/prometheus.yml' 

vim /etc/prometheus/prometheus.yml

On the 'scrape_configs' option, you may need to add monitoring jobs

The default configuration comes with the default monitoring job name 'prometheus' and the target server 'localhost' through the 'static_configs' option.

Change the target from 'localhost:9090' to the server IP address '192.168.1.10:9090' as below.

Note:
Scrape configuration containing exactly one endpoint to scrape:
Here it's Prometheus itself.
scrape_configs:
The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
job_name: "prometheus"

metrics_path defaults to '/metrics'
scheme defaults to 'http'.

static_configs:
targets: ["192.168.1.10:9090"]

4. Change the configuration and data directories to the user 'promethues'.

sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus

Basic prometheus installation finished, Hopefully 👍 .

Configure Prometheus

1. Create a new systemd service 
sudo vim /etc/systemd/system/prometheus.service

Copy and paste the following configuration.

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target

2. Reload the systemd manager to apply a new config.

sudo systemctl daemon-reload

3. Start and enable the Prometheus service

sudo systemctl enable --now prometheus
sudo systemctl status prometheus

Prometheus monitoring tool is now accessible on the TCP port '9090.

4. Visit IP address with port '9090' 

http://192.168.1.10:9090/

And you will see the prometheus dashboard query below.

Prometheus query dashboard

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

