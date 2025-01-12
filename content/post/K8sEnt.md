+++
author = "Hugo Authors"
title = "ProLUG Talk: Kubernetes in the Enterprise"
date = "2025-01-11"
description = "Notes and pontification regarding a talk by Michael Pesa of Lambda Labs held in ProLUG"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG", "Operating Systems", "Kubernetes", "Talos", "IaC", "GitOps", "DevOps", "HomeLab", "Immutable Infrastructure"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Kubernetes in Enterprise

As a dedicated member of the Professional Linux User Group, I gain valuable insights into essential industry tools, processes, and procedures from professional engineers who work hands-on with major infrastructure.

This evening, Michael Pesa of Lambda Labs delivered an excellent talk on best practices with Kubernetes and GitOps, shedding light on the challenges faced by traditional orchestration approaches. What intrigued me most was the discussion on Talos OS and Chainguard, particularly their use of Software Bill of Materials (SBOM). The concept centers around stripping systems down to their bare essentials, which not only reduces vulnerabilities but also improves performance.

Talos OS is particularly fascinating because it eliminates many traditional system components like SSH, systemd, glibc, package managers, or a shell. Essentially, Talos is just the Linux kernel with several Go binaries. This streamlined approach significantly reduces vulnerabilities and minimizes the attack surface. As Michael mentioned in his presentation, many vulnerabilities stem from privilege escalation, container escapes, and memory hacking. Talos mitigates most of these threats by enforcing API-driven controls instead of relying on a shell and by utilizing private key-based authentication throughout.

I am excited to experiment with these tools in my homelab, where I aim to create a modern, declarative infrastructure with ephemerality at its core.

---

# 📝 Notes from the Presentation:

## Topic Covered

### Immutable operating systems
### Minimalist container images
### GitOps strategies
### Reproducible builds

## SUSE MicroOS 🦎
**Purpose:**

- Designed as a container host OS.

**Features:**

- Read-only root filesystem for enhanced security.
- Transactional updates managed via Btrfs snapshots.
- Cloud environment integration with cloud-init; uses ignition elsewhere.

**Usage:**

- Ideal for minimal, secure containerized workloads.

## Talos Linux 🦅

### Overview:

- Minimalist design focusing on immutability.
- Linux kernel paired with five Go binaries.
- Lacks traditional components: no SSH, systemd, glibc, package manager, or shell.

### Security:

- Secure by default with a micro TLS stack and key-based authentication.
- API-driven operations using YAML, akin to Kubernetes manifests.
- Uses its own PKI infrastructure, creating a small attack surface.

### Tools:

- talosctl CLI for management.
- Debugging occurs via ephemeral tools and remote APIs.

### Considerations:

- Best suited for Kubernetes clusters.
- For bare-metal installations, use Matchbox or a PXE boot system.

### Alternatives:

Flatcar and CoreOS are earlier container-focused OS derivatives.

## Minimal Container Images 👝

### Philosophy:

- Containers are inherently immutable; minimal images reduce attack surfaces and improve performance.

### Best Practices:

- Use the smallest possible image, minimizing unnecessary dependencies.
- Employ multi-stage builds for languages with heavy dependencies, separating build and runtime environments.
- For statically compiled languages like Go, containers can often be reduced to a single binary.

### Security:

- Avoid including shells to reduce exploit vectors.
- Favor minimal base images like Alpine or tools like Google’s Distroless and Chainguard’s SBOM-integrated containers.

## Supply Chain Security 🔗🔐

### SBOM (Software Bill of Materials):

- A detailed list of libraries, packages, versions, and licenses within a container image.
- Machine-readable format for automated security checks.

### Software Attestation:

- Authenticated metadata about an artifact (e.g., a container image).
- Enables verification of the artifact’s integrity.

### Chainguard:

- Offers streamlined, dependency-minimized packages.
- Main drawback: constant updates unless on a paid plan.

## Challenges in Immutable Environments ♻️

### Limitations:

- Lack of SSH access and debugging tools on nodes and pods.
- Ephemeral, short-lived infrastructure and read-only filesystems.

### Risks:

- IaC misconfigurations can cause widespread outages.

### Mitigation:

- Implement proper GitOps workflows.
- Ensure consistency between development, staging, and production environments.

## Observability Strategies in Immutable Environments 👀

### Approaches:

- Centralized logging for better insights.
- Monitor clusters and services rather than individual pods or nodes.
- Use ephemeral debugging pods or sidecars with shared access.
- Declarative observability configurations (e.g., using Kubernetes CRDs).

### Principle:

- Treat nodes and pods as “cattle, not pets” to maintain scalability and consistency.

## Key Principles of GitOps and Declarative Infrastructure 😰

### GitOps Core Tenets:

- Git as the single source of truth.
- Automated reconciliation with tools like ArgoCD or Flux.
- Infrastructure changes are auditable and reversible.
- Prevents unauthorized and ad-hoc changes.

### Homelab Use:

- Start with a Talos system in Docker/Podman for experimentation.

## GitOps Challenges 😰

### Common Issues:

- Subtle configuration drift across environments.
- Risk of manual changes not being committed back to Git.
- Managing rollouts from dev to staging to production.
- Scaling to hundreds of clusters.
- Variations in deployment strategies across teams.
- Securely managing secrets outside Git (e.g., Kubernetes Secrets, HashiCorp Vault).

## The GitFlow Workflow 💨

### Workflow Overview:

- Developers create long-lived feature branches.
- Separate primary branches for development, hotfixes, and releases.
- The trunk (dev) branch isn’t always stable or deployable.
- Multi-environment deployments often use separate release branches.

### Complexity:

- Complex merging strategies, especially for hotfixes.
- Tools like ArgoCD or Flux can automate deployments.

## Trunk-Based Development 🐘

### Overview:

- Continuous integration with short-lived feature branches.
- Frequent commits directly to the main branch.
- Emphasizes small, incremental changes to reduce merge conflicts.

### Advantages:

- Simplifies CI/CD pipelines.
- Encourages fast feedback and rapid delivery.

### Tools:

- Works seamlessly with automated systems like Kubernetes or Terraform.

---

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis


