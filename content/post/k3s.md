+++
author = "Hugo Authors"
title = "Why K8S matters"
date = "2024-12-29"
draft = "false"
description = "Despite some of the jokes or criticisms you have heard, Kubernetes matters, here is how I got started"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "Kubernetes", "K3S", "Infrastructure", "Dev Ops", "Platform Engineering", "Rancher"
]
categories = [
    "Experience", "Learning"
]
series = ["Experience"]

+++

<!--more-->

# Kubernetes is Important 🌐🐳

Despite the jokes or criticisms you may have heard, Kubernetes matters a lot. Once I understood the "Why," I became much more motivated to learn the "How." This is how I got started with Kubernetes using my Proxmox Home-Lab and K3S. 

Firstly, I would like to illuminate the "Why," as it’s an important philosophy to grasp before diving in. I firmly believe in understanding the "Why" before the "How." 🧠

## A Brief History of Infrastructure 🕰️

### The Evolution 💾

Many moons ago 🌛, internet infrastructure relied on operating systems to run services. Unix and Linux were preferred because they are multi-user environments suitable for serving files to requesters. In fact, the first implementation of TCP was written on a UNIX system running on a NextSTEP computer. 🖥️ This model worked for decades: more users meant more machines. However, issues arose. Machines would go down, causing cascading effects. ⚠️ Misconfigurations wreaked havoc, and machines often ran inefficiently, either wasting resources or straining hardware. 🔧

### In Comes Virtualization 💻✨

Virtualization revolutionized infrastructure by allowing computers to be divided into independent virtual machines (VMs). A VM is a fully self-contained operating system. This innovation enabled more efficient utilization of hardware, increased flexibility, and reduced downtime caused by hardware failures. 🚀

### Containers Changed the Game 🐳📦

Containers brought another layer of efficiency and standardization. Unlike VMs, containers share the host operating system’s kernel but encapsulate applications and their dependencies. This reduces overhead and enables applications to run consistently across different environments. 🌍 Developers could now "build once, run anywhere," making containers a key tool in modern infrastructure. 🛠️

### Orchestration Was Needed 🤖🎛️

As the use of containers exploded, managing them became increasingly complex. Deploying, scaling, monitoring, and maintaining hundreds or thousands of containers manually was impractical. This is where orchestration tools like Kubernetes stepped in, automating these tasks and ensuring applications are always running, balanced, and recoverable in case of failures. ✅

## The Why 🤔

Understanding the "Why" is key to appreciating Kubernetes' value. Here are some of the core reasons:

- **Monitoring** 📊: Kubernetes provides tools and integrations to monitor your workloads, ensuring you can observe application health and performance in real time.
- **Logging** 📝: Centralized logging in Kubernetes makes it easy to trace and debug issues across distributed systems.
- **Security** 🔒: Kubernetes enhances security through role-based access control (RBAC), network policies, and automatic updates, reducing vulnerabilities in production systems.
- **Ephemerality** 🌀: Kubernetes embraces the concept of ephemeral workloads, where containers can be replaced automatically if they fail, ensuring high availability.
- **Reproducibility** 🔄: Kubernetes enables reproducible deployments by using declarative configurations, allowing you to deploy the same infrastructure consistently across environments.

By addressing these challenges, Kubernetes transforms the way infrastructure is managed and applications are deployed, making it a cornerstone of modern cloud-native computing. ☁️

### During Week 10 of the ProLUG Course [^1] 🛠️📚

John Champine[^2], an OpenShift Engineer, delivered a compelling two-hour presentation on Kubernetes and OpenShift. His anecdotes and technical insights were especially engaging, offering both rich historical context drawn from his personal experience and intricate details about shared resource management. 🖥️⚙️

## My Experience 💻🔧

I heavily utilize Proxmox VE to build out simulated production environments where I can practice various administrative and engineering tasks. In my homelab, I installed K3S and Talos to create a typical dev/testing/production environment. 🌐 One particularly unique workflow I used involved building custom Podman containers—yes, Podman! 🐋 

It’s not widely known that `podman play kube` [^3] can create a manifest for use in Kubernetes pods. With this method, I could prototype and build out containers, functionally test them, and then publish them for declarative deployment. This approach felt incredibly slick to me, as bugs were ironed out during the process, and the final deployment was straightforward. ✅🛠️

Most of my work was done with K3S[^4], a Rancher[^5]-based distro designed for low-resource environments. However, I also experimented with Talos OS, setting up multiple virtual machines in a configuration resembling a multi-machine/node environment—with a sprinkle of jank to keep things interesting. 🤖✨

This hands-on approach allowed me to deepen my understanding of Kubernetes while also refining workflows that integrate containerization and orchestration. 🚀

## Respect 🙌

From these experiences, I have developed a deep respect for Kubernetes. I see it as the operating system of the internet—an innovation that will inspire other similar systems. 🌐 While newer technologies like MicroVMs and hybrid container/VM architectures are emerging, I believe they can be easily incorporated into orchestration schemes like Kubernetes. 🤖🛠️

Given this perspective, I think Kubernetes will remain relevant for a long time, much like Unix/Linux. 🐧 It simply makes sense given the strenuous demands of the modern internet and the ever-growing number of attacks and incidents. Such a resilient system enables greater efficiency, enhanced security, and improved situational awareness for everyone. 🔒🚀

## Footnotes

[^1]: The above quote is excerpted from an earlier BlogPost [Post](trevorsmale.github.io/techblog/post/pacu10/) techblog, 2024.
[^2]: John Champine [Profile](https://www.linkedin.com/in/john-champine-2ba878114?trk=people-guest_people_search-card) LinkedIn, 2024.
[^3]: Podman Export [Docs](https://docs.podman.io/en/v3.4.1/markdown/podman-play-kube.1.html) Podman Docs, 2019.
[^4]: K3S Website [Site](https://k3s.io/) Site, 2025.
[^5]: Rancher Website [Site](https://www.rancher.com/) Site, 2025.

