# 🚀 Cloud & DevOps Engineering Master Notes

[![DevOps Lifecycle](https://img.shields.io/badge/Domain-Cloud%20%26%20DevOps-blue?style=for-the-badge)](https://github.com/vaibhavk2000/My-Cloud-Devops-Notes)
[![Status](https://img.shields.io/badge/Status-Comprehensive%20Guide-success?style=for-the-badge)](https://github.com/vaibhavk2000/My-Cloud-Devops-Notes)

Comprehensive, production-ready engineering notes, architectures, and deep-dive references covering modern cloud-native development, infrastructure automation, CI/CD pipelines, and site reliability engineering (SRE).

---

## 📑 Table of Contents

1. [Introduction to DevOps & Version Control](#1-introduction-to-devops--version-control)
2. [Linux Administration & Networking Basics](#2-linux-administration--networking-basics)
3. [Amazon Web Services (AWS) Architecture](#3-amazon-web-services-aws-architecture)
4. [Containerization (Docker)](#4-containerization-docker)
5. [Container Orchestration (Kubernetes)](#5-container-orchestration-kubernetes)
6. [Infrastructure as Code (Terraform)](#6-infrastructure-as-code-terraform)
7. [CI/CD & Pipeline Engineering (Jenkins)](#7-cicd--pipeline-engineering-jenkins)
8. [Observability & Monitoring (Datadog)](#8-observability--monitoring-datadog)

---

## 1. Introduction to DevOps & Version Control

* **DevOps Culture & Philosophy:** A cultural mindset and practice combining software development (`Dev`) and IT operations (`Ops`). Its core goal is to shorten the systems development life cycle and provide continuous delivery with high software quality.
* **C.A.L.M.S. Framework:** The foundational pillars of DevOps:
  * **C**ulture: Fostering collaboration and shared responsibility.
  * **A**utomation: Eliminating manual work (toil).
  * **L**ean: Eliminating waste and optimizing flow.
  * **M**easurement: Tracking KPIs and metrics.
  * **S**haring: Promoting a transparent, learning-oriented environment.
* **Source Code Management (Git):** A distributed version control system that tracks history, changes, and versions of source code files, allowing developers to revert mistakes and manage parallel work streams.
* **Collaboration via GitHub:** A cloud-based hosting platform for Git repositories that introduces social coding workflows like Pull Requests (PRs), code reviews, issues, and branch protection rules.

---

## 2. Linux Administration & Networking Basics

* **Essential System Commands (`ls`, `cd`, `chmod`):** Command-line primitives used to navigate filesystems (`ls` to list, `cd` to change directory) and modify file access permissions (`chmod`).
* **Secure Access via SSH (Secure Shell):** A cryptographic network protocol used to execute terminal commands and manage remote servers securely over an unsecured network.
* **Web Server Deployment (Apache / Nginx):** Installing and running open-source HTTP servers to host and deliver web content, static files, and reverse-proxy traffic.
* **Networking Foundations & CIDR:** Understanding IP addressing, subnet masks, and **Classless Inter-Domain Routing (CIDR)** notation (e.g., `10.0.0.0/16`) to efficiently define IP address ranges and network boundaries.

---

## 3. Amazon Web Services (AWS) Architecture & Core Services

### Foundations & Compute
* **Cloud Paradigm & Virtualization:** Moving away from physical data centers. Virtualization abstracts underlying hardware using hypervisors, empowering **IaaS** (Infrastructure as a Service), **PaaS** (Platform as a Service), and **SaaS**.
* **Amazon EC2 (Elastic Compute Cloud):** Virtual servers in the cloud allowing you to run applications on demand.
* **Instance Types & Pricing Models:** Tailoring compute power using instance families (General Purpose, Compute/Memory Optimized) and saving costs via On-Demand, Savings Plans, or cost-effective **Spot Instances** (spare capacity).
* **EC2 Dashboard & AMIs:** Managing networking, Elastic IPs, and **Amazon Machine Images (AMIs)**—pre-configured templates containing an OS and software to launch instances instantly.
* **AWS Lambda:** A serverless compute service that executes code in response to events (like file uploads to S3) without requiring you to provision or manage servers.

### Storage Ecosystem
* **Amazon EBS (Elastic Block Store):** High-performance block storage volumes attached to individual EC2 instances, secured and backed up via automated **Snapshots**.
* **Amazon EFS (Elastic File System):** A fully managed, scalable network file system (NFS) that lets multiple EC2 instances share files concurrently.
* **Amazon S3 (Simple Storage Service):** Highly scalable object storage organized into **Buckets**, using storage classes and automated lifecycle policies to minimize costs.
* **Amazon CloudFront:** A fast content delivery network (CDN) globally caching and accelerating the delivery of web content stored in S3 or EC2.

### Networking & Security Architecture
* **VPCs (Virtual Private Clouds):** Isolated virtual networks dedicated to your AWS account, split into subnets using custom **Route Tables**.
* **Internet & NAT Gateways:** Components allowing instances in private subnets to reach the outside internet safely (`NAT Gateway`), or connecting the VPC to the public internet (`Internet Gateway`).
* **Security Groups vs. NACLs:** Defense-in-depth security layers. **Security Groups** act as stateful firewalls at the instance (ENI) level, while **Network ACLs** act as stateless firewalls at the subnet level.
* **Elastic Network Interfaces (ENIs):** Virtual network cards that can be attached/detached from EC2 instances to manage IPs and MAC addresses.
* **Amazon GuardDuty:** An intelligent threat detection service that continuously monitors AWS accounts and workloads for malicious activity.
* **Elastic Load Balancers (ALB/NLB):** Distributing incoming application traffic across multiple targets (like EC2 instances) to ensure high availability, paired with **SSL termination** for encrypted web traffic.
* **Auto Scaling:** Automatically adding or removing compute capacity based on defined traffic loads and performance metrics.
* **Route 53:** A scalable cloud Domain Name System (DNS) web service providing domain registration and global traffic routing policies.
* **IAM (Identity and Access Management):** Managing granular permissions and secure access using Users, Groups, Roles, JSON-based **Policies**, Multi-Factor Authentication (MFA), and the **Principle of Least Privilege**.

### Data, Automation & Observability
* **Amazon RDS (Relational Database Service):** Managed SQL database engines (MySQL, PostgreSQL) handling patching, backups, and **Multi-AZ** high-availability replication.
* **AWS CLI:** Command-line tool used to script, automate, and manage AWS resources directly from your terminal.
* **Amazon CloudWatch:** Monitoring service tracking metrics, logs, visual dashboards, and custom alarms integrated with **SNS (Simple Notification Service)** for instant email/SMS alerts.

---

## 4. Containerization (Docker)

* **Containerization & Local Containers:** Packaging an application together with its dependencies into an isolated container package that can run consistently anywhere.
* **Images & Registries (DockerHub / ECR):** Read-only templates used to create containers, stored and shared via registries like DockerHub or Amazon Elastic Container Registry (**ECR**).
* **Docker Networking:** Mechanisms allowing containers to communicate with each other, host machines, and external networks safely via bridges and overlays.
* **Docker Volumes:** The preferred mechanism for persisting data generated by and used by Docker containers independently of the container lifecycle.
* **Dockerfiles & Multi-Stage Builds:** Text documents containing sequential commands to assemble a Docker image. Multi-stage builds separate heavy compilation environments from lean runtime environments to minimize image size and security footprints.
* **Docker Compose:** A tool for defining and running multi-container Docker applications using a single YAML configuration file.

---

## 5. Container Orchestration (Kubernetes)

* **Cluster Architecture (Control Plane vs. Worker Nodes):** Kubernetes clusters consist of a **Control Plane** (managing state, scheduling, API server) and **Worker Nodes** (running actual container workloads via `kubelet`).
* **AWS EKS (Elastic Kubernetes Service):** A managed Kubernetes service provided by AWS that eliminates the complexity of running your own control plane.
* **Kubernetes Networking:** Flat network architecture enabling every pod to have its own unique IP address, allowing direct communication across nodes without NAT.
* **YAML Manifests:** Declarative configuration files defining the desired state of Kubernetes objects (deployments, pods, services).
* **Deployments vs. StatefulSets:** **Deployments** manage stateless replica applications with rolling updates, while **StatefulSets** manage stateful applications requiring unique network identifiers and persistent storage mapping.
* **Configuration & Storage (ConfigMaps/Secrets/Volumes):** Decoupling configuration data and sensitive secrets from container images, alongside Persistent Volume Claims (PVCs) for storage.
* **Horizontal Pod Autoscaler (HPA):** Automatically scales the number of pod replicas up or down based on observed CPU utilization or custom metrics.
* **Ingress & Load Balancing:** Managing external access to services inside a cluster via HTTP/HTTPS routing rules and entry point controllers.
* **Three-Tier Application Deployment:** Orchestrating a complete architecture composed of a Frontend layer, Backend API layer, and Database layer running inside Kubernetes pods.

---

## 6. Infrastructure as Code (Terraform)

* **IaC Principles & Syntax:** Managing and provisioning computing infrastructure through human-readable configuration files rather than manual interactive tool click-ops.
* **State Management:** Tracking real-world resource mappings inside a state file, securely stored remotely using S3 backends paired with **DynamoDB** state locking to prevent write conflicts.
* **Security & User Data:** Automating security group configurations and passing initialization scripts (`user_data`) to provisioned servers.
* **Modules & Dependencies:** Packaging reusable groups of Terraform resources into **Modules**, and managing explicit or implicit resource dependency ordering.
* **Workspaces & Loops:** Managing multiple isolated environments (dev, staging, prod) using workspaces, and writing dynamic blocks utilizing loops (`count`, `for_each`).
* **Terraform Commands & Provisioners:** Core CLI operations (`init`, `plan`, `apply`, `destroy`), along with provisioners used to execute scripts on local or remote machines during creation.
* **Deploying EKS with Terraform:** Utilizing code modules to dynamically provision a fully functional AWS EKS cluster infrastructure.

---

## 7. CI/CD & Pipeline Engineering (Jenkins)

* **CI/CD Fundamentals:** **Continuous Integration** (frequently merging code changes into a central repository followed by automated builds/tests) and **Continuous Delivery/Deployment** (automatically releasing changes to production).
* **Jenkins Architecture:** A Java-based automation server utilizing a **Controller-Agent** model to distribute heavy build loads across isolated build nodes.
* **Build Tools (Maven):** A build automation tool primarily used for Java projects to manage project object models (POM), dependencies, and compilation.
* **Code Quality (SonarQube):** Automated static code analysis tool used to detect bugs, security vulnerabilities, and code smells in pipelines.
* **Declarative Jenkins Pipelines:** Describing continuous delivery pipelines using a structured Groovy-based syntax inside a `Jenkinsfile`.
* **Artifact Storage & Three-Tier Deployment:** Archiving compiled build artifacts (like `.jar` or `.war` files) and orchestrating end-to-end automated deployments of a multi-tier app directly onto an EKS cluster.

---

## 8. Observability & Monitoring (Datadog)

* **Infrastructure & App Monitoring:** Integrating agent-based data collection to gather metrics, distributed traces, and log files from cloud servers, containers, and application runtimes.
* **Alerting & Dashboards:** Building real-time graphical visibility dashboards and setting up alerting monitors to catch production issues proactively.
