# 🚀 Cloud & DevOps Engineering Master Notes

Comprehensive, production-ready engineering notes, architectures, and deep-dive references covering modern cloud-native development, infrastructure automation, and site reliability engineering (SRE).

---

## 📑 Table of Contents

1. [DevOps Fundamentals & C.A.L.M.S.](https://www.google.com/search?q=%25231-devops-fundamentals--calms&utm_source=gemini)
2. [Linux Administration & Engineering](https://www.google.com/search?q=%25232-linux-administration--engineering&utm_source=gemini)
3. [Amazon Web Services (AWS) Architecture](https://www.google.com/search?q=%25233-amazon-web-services-aws-architecture&utm_source=gemini)
4. [Containerization (Docker)](https://www.google.com/search?q=%25234-containerization-docker&utm_source=gemini)
5. [Container Orchestration (Kubernetes)](https://www.google.com/search?q=%25235-container-orchestration-kubernetes&utm_source=gemini)
6. [Infrastructure as Code (Terraform)](https://www.google.com/search?q=%25236-infrastructure-as-code-terraform&utm_source=gemini)
7. [CI/CD & Pipeline Engineering (Jenkins)](https://www.google.com/search?q=%25237-cicd--pipeline-engineering-jenkins&utm_source=gemini)

---

## 1. DevOps Fundamentals & C.A.L.M.S.

* **Core Philosophy:** Breaking down silos between development and operations to optimize software delivery velocity and stability.
* **The 5 Pillars (C.A.L.M.S.):**
* **C**ulture: Shared responsibility and blameless post-mortems.
* **A**utomation: Eliminating manual toil across builds, tests, and provisioning.
* **L**ean: Minimizing work-in-progress (WIP) and eliminating waste.
* **M**easurement: Tracking technical KPIs and business outcomes.
* **S**haring: Documenting workflows and spreading team insights.


* **DORA Metrics:** Deployment Frequency, Lead Time for Changes, Change Failure Rate, and Mean Time to Recovery (MTTR).

---

## 2. Linux Administration & Engineering

* **Architectural Layers:** User Space $\rightarrow$ System Call Interface (SCI) $\rightarrow$ Kernel Space $\rightarrow$ Hardware.
* **FHS Highlights:**
* `/etc`: Host-specific static configuration files.
* `/var`: Dynamic runtime logs (`/var/log`) and caches.
* `/proc`: Real-time in-memory kernel pseudo-file system.


* **Permissions & Security:** Octal permission calculations (`chmod 754`) alongside advanced bits like **SUID** (4000), **SGID** (2000), and the **Sticky Bit** (1000).
* **Text Processing Pipeline:** Streamlined usage of `grep`, `awk`, and `sed` for log analysis and configuration updates.

---

## 3. Amazon Web Services (AWS) Architecture

* **Compute (EC2):** Nitro System hypervisor optimization profiles ranging from General Purpose (`M`/`T`), Compute Optimized (`C`), to Memory Optimized (`R`/`X`). Pricing strategies cover On-Demand, Savings Plans, and cost-saving Spot Instances.
* **Storage Matrix:** Strict decoupling between block storage (`EBS`), scalable shared file systems (`EFS`), and object storage (`S3`).
* **VPC Networking:** Dual-subnet enterprise topologies leveraging Internet Gateways, NAT Gateways, stateful Security Groups (ENI-level), and stateless Network ACLs (subnet-level).

---

## 4. Containerization (Docker)

* **Kernel Primitives:** Leveraging Linux **Namespaces** for process, network, and mount isolation, alongside **Control Groups (cgroups)** for strict CPU and memory resource constraints.
* **Production Best Practices:**
* **Multi-Stage Builds:** Separating heavy compilation tools from minimal, secure runtime images.
* **Layer Caching:** Sequencing Dockerfile instructions from least frequently changed (dependencies) to frequently changed (source code).
* **Non-Root Execution:** Running containers under dedicated non-privileged user accounts to minimize attack surfaces.



---

## 5. Container Orchestration (Kubernetes)

* **Control Plane vs. Worker Nodes:** Core interactions between `kube-apiserver`, `etcd`, `kube-scheduler`, `kubelet`, and `kube-proxy`.
* **Resource Governance:** Defining precise CPU/Memory **Requests** (for scheduling) and **Limits** (for hard resource ceilings and OOM protection).
* **Health Probes:** Utilizing **Liveness Probes** for automated failure recovery and **Readiness Probes** to control live traffic routing.

---

## 6. Infrastructure as Code (Terraform)

* **Immutable vs. Mutable:** Deploying reproducible infrastructure changes via clean environment teardowns and rebuilds rather than live in-place server modifications.
* **State Management:** Using remote S3 backends combined with DynamoDB locking tables to prevent concurrent pipeline write conflicts.

---

## 7. CI/CD & Pipeline Engineering (Jenkins)

* **Controller-Agent Architecture:** Offloading heavy build jobs and security footprints away from the central controller to isolated worker nodes.
* **Declarative Pipelines:** Scripted Jenkinsfiles incorporating multi-stage code checkouts, static analysis (`SonarQube`), testing suites (`JUnit`), and automated artifact deployments.

---
