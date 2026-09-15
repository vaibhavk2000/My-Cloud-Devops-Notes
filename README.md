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
* **DevOps Culture & Philosophy:** Breaking down silos between development and operations to optimize software delivery velocity and stability using the **C.A.L.M.S.** framework (Culture, Automation, Lean, Measurement, Sharing).
* **Source Code Management (Git):** Tracking changes, branching strategies, merging, and collaborative workflows.
* **Collaboration via GitHub:** Pull requests, code reviews, issue tracking, and repository management for team-based engineering.

---

## 2. Linux Administration & Networking Basics
* **Essential System Commands:** Fundamental file and system operations utilizing commands like `ls`, `cd`, and `chmod` for permission management.
* **Secure Access & Deployment:** Utilizing `SSH` to connect securely to remote servers (like EC2) and deploying web servers such as Apache or Nginx.
* **Networking Foundations:** Understanding IP addressing, subnetting, and **CIDR** block notation for routing and network design.

---

## 3. Amazon Web Services (AWS) Architecture & Core Services

### Foundations & Compute
* **Cloud Paradigm & Virtualization:** Scaling cloud workloads leveraging IaaS, PaaS, and SaaS built on hypervisor virtualization.
* **Amazon EC2 & Dashboards:** The backbone of AWS compute. Tailoring workloads via instance types, spot/on-demand pricing models, Elastic IPs, and AMIs. Managed with security groups and key pairs.
* **AWS Lambda:** Serverless, event-driven compute running code without managing underlying servers, natively integrating with S3, DynamoDB, and API Gateway.

### Storage Ecosystem
* **Amazon EBS & EFS:** Persistent block storage volumes with automated snapshots, alongside scalable network file systems (EFS) for multi-instance shared storage.
* **Amazon S3 & CloudFront:** Scalable object storage buckets utilizing lifecycle policies, storage classes, and global content delivery acceleration via CloudFront caching.

### Networking & Security Architecture
* **VPCs & Gateways:** Isolated cloud networks defined using CIDR blocks, custom subnets, route tables, Internet Gateways, and NAT Gateways.
* **Network Security Layers:** Defense-in-depth security using stateful Security Groups, stateless NACLs, Elastic Network Interfaces (ENIs), and real-time threat detection via **Amazon GuardDuty**.
* **Traffic Management & High Availability:** Application and Network Load Balancers (ALBs/NLBs) with SSL termination, automated scaling policies, and global DNS management via **Route 53**.
* **Identity & Governance (IAM):** Securing accounts using users, groups, roles, JSON policies, Multi-Factor Authentication (MFA), and the Principle of Least Privilege.

### Data, Automation & Observability
* **Amazon RDS:** Managed relational databases (MySQL/PostgreSQL) with automated backups and Multi-AZ deployments.
* **AWS CLI & CloudWatch:** Scripting deployments from the command line while monitoring performance using metrics, alarms, visual dashboards, and SNS alerting.

---

## 4. Containerization (Docker)
* **Core Concepts:** Introduction to containerization, managing and interacting with local containers.
* **Images & Registry:** Working with Docker images, tagging, pushing/pulling via DockerHub or AWS ECR.
* **Networking & Storage:** Understanding Docker network bridges/drivers and persistent data management with Docker Volumes.
* **Builds & Compose:** Writing optimized `Dockerfiles` using multi-stage builds, and orchestrating multi-container applications locally using **Docker Compose**.

---

## 5. Container Orchestration (Kubernetes)
* **Cluster Foundations:** Architecture overview (Control Plane vs. Worker Nodes), setting up Kubernetes on AWS via **EKS**, and networking fundamentals.
* **Manifests & Workloads:** Writing YAML manifests to manage workloads including **Deployments** and **StatefulSets**.
* **Configuration & Storage:** Managing configmaps, secrets, and persistent storage volumes.
* **Traffic & Scaling:** Configuring Horizontal Pod Autoscaling (HPA), Ingress controllers, load balancing, and deploying complete **Three-Tier Applications** on Kubernetes.

---

## 6. Infrastructure as Code (Terraform)
* **IaC Principles:** Transitioning to immutable infrastructure management and understanding Terraform syntax.
* **State & Security:** Managing remote Terraform state (S3/DynamoDB backends), security groups, user data scripts, and multi-environment workspaces.
* **Modules & Logic:** Writing reusable Terraform modules, managing dependencies, loops, and advanced conditional blocks.
* **Provisioning & EKS:** Executing lifecycle commands, utilizing provisioners, and automating the deployment of complete cloud footprints and **EKS Clusters**.

---

## 7. CI/CD & Pipeline Engineering (Jenkins)
* **CI/CD Fundamentals:** Introduction to continuous integration and delivery pipelines.
* **Jenkins Architecture:** Setting up Jenkins controllers, managing agents, job configurations, and integrating Git repositories.
* **Build Tools & Quality:** Integrating **Maven** for builds and running static code analysis checks using **SonarQube**.
* **Advanced Pipelines:** Writing declarative Jenkins pipelines, artifact management, and building automated deployments for a **Three-Tier Application on EKS**.

---

## 8. Observability & Monitoring (Datadog)
* **Application & Infrastructure Monitoring:** Integrating infrastructure metrics, traces, and logs into **Datadog**.
* **Alerting & Dashboards:** Setting up custom monitors, service level objectives (SLOs), and real-time operational visibility dashboards.
