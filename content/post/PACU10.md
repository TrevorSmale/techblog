+++
author = "Hugo Authors"
title = "ProLUG Admin Course Unit 10 🐧"
date = "2024-11-17"
description = "ProLUG Admin Course Unit 10"
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

# Kubernetes 

"The OS of the Internet"

This week, we were privileged to host a special guest lecture by John Champine, Scott's brother. Over the course of an engaging two-hour session, John delivered an in-depth exploration of Kubernetes, thoroughly covering the five W's: Who, What, When, Where, and Why. 

John's passion and deep knowledge of Kubernetes were evident throughout the presentation. Having firsthand experience with the challenges of pre-Kubernetes infrastructure, he offered valuable insights into how this platform has revolutionized modern computing. Notably, John specializes in OpenShift, an IBM-owned management layer built atop Kubernetes. OpenShift adds additional functionality and ease of use to what is already a powerful but complex system.

One concept that particularly caught my attention was the **fractionalization of CPU and memory resources** made possible by Kubernetes' sophisticated resource management. John introduced the term *millicore*, a concept I was previously unfamiliar with. It refers to the fine-grained allocation of processing power, where fractions of a CPU core are shared across processes during compute cycles. This ability to manage resources at such a granular level is remarkable, showcasing the efficiency and precision of Kubernetes.

Before this lecture, I never imagined that such details—down to the microsecond allocation of core usage—could not only be considered but also controlled and utilized to optimize workloads. This level of resource management truly solidifies Kubernetes' position as the "operating system of the internet," enabling applications to run more efficiently and reliably across diverse infrastructures.

John’s insights not only deepened my understanding of Kubernetes but also sparked curiosity about the broader implications of containerized resource management in modern computing. 

One idea that has been dispelled by doing these exercises is that Kubernetes is overkill for most things. Yes, standing up multiple networked nodes and having them interoperate is no small task. However, there exist lightweight, single node variants like K3s that enable a simplified experience while maintaining the powerful advantages of an orchestration system.

Furthermore, given the threat landscape, desire for declarative infrastructure and ever fluctuating demand, it makes sense to implement a tool that is designed for these modern demands.
---

## Discussion Post 1 

Reading: https://kubernetes.io/docs/concepts/overview/ 📗

### Question 1

**What are the two most compelling reasons you see to implement Kubernetes in your organization?**

1. **Efficient Usage of Resources**: Kubernetes optimizes resource allocation and ensures workloads are distributed efficiently across your infrastructure.
2. **Built-in Redundancy**: Kubernetes provides automatic failover and self-healing capabilities, ensuring high availability for applications.

Other notable reasons include:
- **Load Balancing**: Automatically distributes traffic across pods to maintain application performance. ⚖️
- **Centralized Control**: Manages deployments, updates, and scaling from a single interface. 🎯
- **Declarative in Nature**: Uses declarative configurations, making it easier to maintain and reproduce states. 🔖
- **Customizable for Organizational Needs**: Offers flexibility to adapt to specific requirements through pluggable components. 🔌
- **Open Source**: Eliminates vendor lock-in while benefiting from a large and active community. ⚙️
- **Broadly Used**: Extensive community adoption ensures a wealth of resources, tools, and discussions. 📜
- **Written in Go**: Delivers a lightweight, high-performance foundation that I am irrationally a fan of 😉

---

### Question 2

**When the article says Kubernetes is not a PaaS, what do they mean by that? What is a PaaS in comparison?**

Kubernetes is not a Platform-as-a-Service (PaaS) because it operates at the **container level** rather than the **application or hardware level**. While it provides some features similar to PaaS offerings, Kubernetes emphasizes flexibility, composability, and user choice rather than prescribing a monolithic solution.

#### Key Characteristics of Kubernetes Compared to PaaS:

1. **What Kubernetes Is**:
   - Provides foundational building blocks for deploying, scaling, and managing containerized applications.
   - Is modular, with optional and pluggable solutions for logging, monitoring, alerting, and scaling.
   - Focuses on maintaining desired state through independent, composable control processes.
   - Preserves user choice by not dictating application or configuration specifics.

2. **What Kubernetes Is Not**:
   - Does not limit the types of applications it supports.
   - Does not deploy source code or build your application.
   - Does not provide application-level services (e.g., middleware, databases).
   - Does not enforce specific logging, monitoring, or alerting solutions.
   - Does not include a proprietary configuration system or language (e.g., Jsonnet).
   - Does not manage comprehensive machine-level configuration, maintenance, or self-healing.

#### Comparison to PaaS:
A Platform-as-a-Service (PaaS), such as Heroku or OpenShift, provides a more opinionated, all-encompassing solution by:
- Supporting application deployment directly from source code.
- Managing application-level services like databases or caching layers.
- Offering integrated logging, monitoring, and alerting.
- Providing a simplified developer experience but with less flexibility.

In contrast, Kubernetes gives users the tools to build their own developer platforms while leaving critical choices in the user’s hands.

---

## Discussion Post 2 

### Scenario

You get a ticket about your new test cluster. The team is unable to deploy some of their applications. They suspect there is a problem and send you over this output:

### Kubernetes Cluster Information

#### kubectl Version
| Component          | Version                                      |
|--------------------|----------------------------------------------|
| Client Version     | v1.31.6+k3s3                                |
| Kustomize Version  | v5.0.4-0.20230601165947-6ce0bf390ce3        |
| Server Version     | v1.30.6+k3s1                                |

#### Node Status
| NAME           | STATUS   | ROLES               | AGE  | VERSION        |
|----------------|----------|---------------------|------|----------------|
| Test_Cluster1  | Ready    | control-plane,master| 17h  | v1.30.6+k3s1   |
| Test_Cluster2  | NotReady | worker              | 33m  | v1.29.6+k3s1   |
| Test_Cluster3  | Ready    | worker              | 17h  | v1.28.6+k3s1   |

### Question 1 

What are you checking on the cluster to validate you see their error?

To identify and validate the issue with the node **Test_Cluster2**:

Check the overall cluster status
- Run **kubectl get nodes** to see the status of all nodes in the cluster.
- This confirms **Test_Cluster2** is **NotReady**.

Inspect node details
- Run **kubectl describe nodes Test_Cluster2** to check for events, taints, and resource usage issues. Look for errors related to kubelet, network, or node conditions.

Access the node**
- SSH into the node with **ssh Test_Cluster2** to perform further diagnostics.

Verify kubelet status
- Run **systemctl status kubelet** to check if the kubelet service is running and healthy.

Check container runtime
- Depending on your runtime, run either **systemctl status docker** or **systemctl status podman** to ensure the container runtime is operational.

Reload and restart services
- If issues are detected, attempt to reload the systemd daemon (**systemctl daemon-reload**) and restart kubelet (**systemctl restart kubelet**).

---

### Question 2

What do you think the problem could be?

Potential problems for **Test_Cluster2** being **NotReady** include:

- The **Test_Cluster2** node is running Kubernetes version **v1.29.6+k3s1**, which is older than the server and other nodes (**v1.30.6+k3s1**). This may cause compatibility issues.
- The kubelet service might have failed or not started.
- Networking problems could prevent the node from communicating with the control plane.
- The node may lack sufficient CPU, memory, or disk space for the kubelet to function.
- Taints, labels, or configuration errors might prevent the node from becoming **Ready**.
- Hardware issues or kernel module problems may impact the node's health.

### Question 3 

Do you think someone else has tried anything to fix this problem before you? Why or why not?

Yes, someone may have tried to fix it 
- The node was added only **33 minutes ago**, indicating recent activity.
- It’s common for issues to be noticed and an initial attempt made before escalating.
- **systemctl status** may show if services were restarted recently, suggesting prior troubleshooting.

No, it may not have been addressed yet because:
- The node is still in the **NotReady** state, implying no successful resolution so far.
- Lack of detailed documentation or escalation might mean no one has investigated yet.

To confirm, check the node's event logs (**kubectl describe nodes Test_Cluster2**) or system logs (**journalctl -u kubelet**) for evidence of recent actions.

---

## Discussion Post 3 

### Scenario:

You are the network operations center (NOC) lead. Your team has recently started supporting the dev, test, and QA environments for your company’s K8s cluster. Write up a basic checkout procedure for your new NOC personnel to verify operation of the cluster before escalating on critical alerts. 

# Kubernetes Cluster Checkout Procedure for NOC Personnel

This document outlines the basic steps for verifying the operation of the Kubernetes (K8s) cluster before escalating on critical alerts. 

---

## **1. Verify Cluster Health**

#### - **Login to the Cluster**

   - Access the cluster using the **kubectl** command-line tool.
   - Use the appropriate kubeconfig file for the environment (dev, test, QA).

        kubectl config use-context <context-name>

#### - **Check Cluster Nodes**

        kubectl get nodes

- Look for any nodes in NotReady, Unknown, or SchedulingDisabled status. Investigate any anomalies.

#### - **Check System Pods**

- Verify critical system pods in the kube-system namespace are running

        kubectl get pods -n kube-system

#### Pay special attention to core components:

- kube-apiserver
- kube-scheduler
- kube-controller-manager
- etcd

---

## **2. Verify Namespace and Application Status**

#### - **List All Namespaces**

        kubectl get namespaces

#### - **Check Pod Status in Active Namespaces**

        kubectl get pods -n <namespace>

#### - **Inspect Deployments and ReplicaSets**

        kubectl get deployments -n <namespace>
        kubectl get rs -n <namespace>

---

## **3. Verify Cluster Networking**

#### - **Service Connectivity**

        kubectl get svc -n <namespace>

#### - **DNS Resolution**

        kubectl exec -n kube-system <coredns-pod> -- nslookup kubernetes.default

#### - **Ingress/Load Balancer**

        kubectl get ingress -n <namespace>

---

## **4. Monitor Resource Utilization**

Node Resource Usage

        kubectl top nodes

Pod Resource Usage

        kubectl top pods -n <namespace>

---

## **5. Check Cluster Events**

        kubectl get events --sort-by='.metadata.creationTimestamp'

---

## **6. Validate Logging and Monitoring**

- Ensure logging systems (e.g., EFK, Fluentd) are collecting logs as expected.
- Verify monitoring dashboards (e.g., Prometheus, Grafana) for anomalies or missing metrics.

---

## **7. Escalation Criteria**

- Core system pods (kube-apiserver, etcd) are not running.
- A node remains in NotReady for more than 10 minutes.
- Pods in critical application namespaces are in CrashLoopBackOff or Error state without resolution.
- Networking issues persist and cannot be resolved by basic checks.
- Cluster resource utilization is critically high (e.g., >90% CPU or memory).

---

## **8. Document Findings**

- Record all findings in the incident management system, including:
- Steps taken during the checkout procedure.
- Observed errors or anomalies.
- Commands run and their output (if significant).

## End of Document
---

### What information online helped you figure this out? What blogs or tools did you use?

- https://kubernetes.io/docs/tasks/debug/debug-cluster/
- https://kubernetes.io/docs/concepts/
- From conducting study-groups and working with K3s

### What did you learn in this process of writing this up?

- Kubernetes is an Operating System with similar failure modes to that of Linux
- Kubernetes maintains helpful logs
- Kubernetes gives helpful errors
- KubeCTL is the tool I should internalize and master
- KubeCTL can manage multiple clusters remotely

---

## Digging Deeper ⛏️

1. Work more in the lab to build a container of your choice and then find out how to
deploy that into your cluster in a secure, scalable way. 👍

I have actually experimented with building out custom containers using Podman and using:

        podman commit

to export the image as a custom local image. Then, I run that image and input the command:

        podman generate kube my-container > my-container.yaml

This creates a manifest yaml that can then be spun up with Kubernetes.

2. Read this about securing containers: https://docs.docker.com/build/building/best-
practices

I do have prior experience with this having completed the DevSecOps certification from TryHackMe, it covers containers security with red-team exercises.
However, it does not cover a ton of defensive measures. This best practices document is going to be really helpful when setting up future containers.

- Do this to practice securing those containers. https://killercoda.com/killer-
shell-cks/scenario/static-manual-analysis-docker 👍
 
3. Read these about securing Kubernetes Deployments:

https://kubernetes.io/docs/concepts/security/ 👍 and
https://kubernetes.io/docs/concepts/security/pod-security-standards/

- TLS
- Secrets API
- Runtime Classes
- Network Policies
- Admission Policy
- Pod Security standards
- Open Policy, assumed safe privileged user.
- Baseline Policy
- SELinux enforce Policy

- Do this lab to practice securing Kubernetes: https://killercoda.com/killer-
shell-cks/scenario/static-manual-analysis-k8s

So checking the privilege status indicates a container is privleged or not. If privleged, move to insecure directory.


---

# Labs 🥼🧪

### Warmup

        curl -sfL https://get.k3s.io > /tmp/k3_installer.sh 👍
        
        more /tmp/k3_installer.sh 👍

What do you notice in the installer? 

What checks are there?

        grep -i arch /tmp/k3_installer.sh

What is the name of the variable holding the architecture?

How is the system finding that variable?

        uname -m

Verify your system architecture
        
        grep -iE “selinux|sestatus” /tmp/k3_installer.sh

Does K3s check if selinux is running, or no? Yes 👍


### Installing k3s and looking at how it interacts with your Linux system

Install k3s
        curl -sfL https://get.k3s.io | sh -

What was installed on your system?

        rpm -qa –last | tac

Your tasks in this lab are designed to get you thinking about how container deployments interact with

3. Verify the service is running

        systemctl status k3s

4. Check it systemd configuration

        systemctl cat k3s

5. See what files and ports it is using

        ss -ntulp | grep <pid from 3>

        lsof -p <pid from 3>

#Do you notice any ports that you did not expect?

6. Verify simple kubectl call to API

        kubectl get nodes

7. Verify K3s is set to start on boot and then cycle the service

        systemctl is-enabled k3s

        systemctl stop k3s

Recheck your steps 3-6

What error do you see, and is it expected?

What is the API port that kubectl is failing on?

        systemctl start k3s

Verify your normal operations again.

Looking at the K3 environment

1. Check out components

        kubectl version

        kubectl get nodes
        
        kubectl get pods -A
        
        kubectl get namespaces
        
        kubectl get configmaps -A
        
        kubectl get secrets -A

#Which namespace seems to be the most used?

Creating Pods, Deployments, and Services

It’s possible that the lab will fail in this environment. Continue as you can and identify the problem using

the steps at the end of this section.

1. Create a pod named webpage with the image nginx

        kubectl run webpage --image=nginx

2. Create a pod named database with the image redis and labels set to tier=database

        kubectl run database --image=redis --labels=tier=database
3. Create a service with the name redis-service to expose the database pod within the cluster on port

        6379 (default Redis port)
        kubectl expose pod database --port=6379 --name=redis-service --
        type=ClusterIP

4. Create a deployment called web-deployment using the image nginx that has 3 replicas
        
        kubectl create deployment web-deployment --image=nginx --
        replicas=3

5. Verify that the pods are created

        kubectl get deployments
        kubectl get pods

6. Create a new namespace called my-test

        kubectl create namespace my-test

7. Create a new deployment called redis-deploy with the image redis in your my-test namespace with 2

        replicas
        kubectl create deployment redis-deploy -n my-test --image=redis -
        -replicas=2

Do some of your same checks from before. What do you notice about the pods you created? Did they all
work?

If this breaks in the lab, document the error. Check your disk space and RAM, the two tightest
constraints in the lab. Using systemctl restart k3s and journalctl -xe can you figure out what is failing?
(Rocky boxes may have limitations that cause this to not fully deploy, can you find out why?)

---

## Reflection questions:

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


[^1]: MITRE | ATT&CK [Site](https://attack.mitre.org/) Knowledge Base.