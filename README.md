# 📘 DevOps & Cloud Engineering Notes

## 1. DevOps Overview
- **Definition**: DevOps = Development + Operations → culture + practices for faster delivery & reliability.
- **Principles**: CI/CD, Automation, Collaboration, Monitoring.

### 🔄 DevOps Workflow
```mermaid
flowchart LR
    A[Code Commit] --> B[Build & Test]
    B --> C[Containerization - Docker]
    C --> D[Orchestration - Kubernetes]
    D --> E[IaC - Terraform/Ansible]
    E --> F[Deployment - AWS/GCP/Azure]
    F --> G[Monitoring - Prometheus/Grafana]
```

---

## 2. Linux
- **Definition**: Open‑source OS based on Unix.
- **Architecture**:
```mermaid
flowchart TD
    A[Applications] --> B[Shell]
    B --> C[System Utilities]
    C --> D[System Libraries]
    D --> E[Kernel]
    E --> F[Hardware]
```

---

## 3. AWS (Amazon Web Services)
- **Definition**: Largest cloud platform (200+ services).
- **Core Services**: EC2, S3, RDS, VPC, CodePipeline, CloudFormation.

### ☁️ AWS Service Categories
```mermaid
graph TD
    A[AWS] --> B[Compute - EC2/Lambda]
    A --> C[Storage - S3/EBS]
    A --> D[Databases - RDS/DynamoDB]
    A --> E[Networking - VPC/Route53]
    A --> F[DevOps Tools - CodePipeline]
```

---

## 4. Git
- **Definition**: Distributed version control system.
- **Workflow**:
```mermaid
flowchart LR
    A[Working Directory] --> B[Staging Area - git add]
    B --> C[Local Repo - git commit]
    C --> D[Remote Repo - git push/pull]
```

---

## 5. GitHub vs GitLab
| Aspect       | GitHub                          | GitLab                          |
|--------------|---------------------------------|---------------------------------|
| Focus        | Collaboration, open‑source      | End‑to‑end DevOps lifecycle     |
| CI/CD        | GitHub Actions                  | Native CI/CD built‑in           |
| Community    | Largest developer base          | Smaller, enterprise‑focused     |
| Hosting      | Cloud‑based                     | Cloud + self‑hosting option     |

---

## 6. Docker
- **Definition**: Containerization platform.
- **Workflow**:
```mermaid
flowchart LR
    A[Code] --> B[Dockerfile]
    B --> C[Build Image]
    C --> D[Run Container]
    D --> E[Deploy]
```

---

## 7. Kubernetes (K8s)
- **Definition**: Container orchestration platform.
- **Workflow**:
```mermaid
flowchart TD
    A[kubectl apply] --> B[API Server]
    B --> C[etcd - store config]
    B --> D[Scheduler]
    D --> E[Assign Pod to Node]
    E --> F[Kubelet - run container]
    F --> G[Kube-proxy - networking]
```

---

## 8. Interview Quick Prep
- **DevOps**: “Culture + practices for faster delivery, reliability, and collaboration.”
- **Linux vs Windows**: “Linux is open‑source, CLI‑driven, stable; Windows is proprietary, GUI‑driven.”
- **AWS EC2 vs Lambda**: “EC2 = virtual servers; Lambda = serverless functions.”
- **Git vs SVN**: “Git stores snapshots, distributed; SVN stores deltas, centralized.”
- **GitHub vs GitLab**: “GitHub = collaboration; GitLab = full DevOps lifecycle.”
- **Docker vs VM**: “VMs emulate hardware; Docker containers share OS kernel → lightweight.”
- **Kubernetes vs Docker**: “Docker = containerization; Kubernetes = orchestration of containers.”
