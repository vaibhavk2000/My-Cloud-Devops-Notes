# 📘 DevOps & Cloud Engineering Notes (Complete Guide)

## 1. DevOps
**Definition**: DevOps is a culture and practice that integrates software development and IT operations. It emphasizes automation, CI/CD, collaboration, and monitoring to deliver applications faster and more reliably.

## 🔄 DevOps Workflow
* Code Commit
* Build & Test
* Containerization - Docker
* Orchestration - Kubernetes
* IaC - Terraform/Ansible
* Deployment - AWS/GCP/Azure
* Monitoring - Prometheus/Grafana

---

## 2. Linux
**Definition**: Linux is an open-source operating system based on Unix. It provides stability, security, and flexibility, making it the backbone of servers, cloud platforms, and DevOps environments.

## 🖥️ Linux Architecture
* Applications
* Shell
* System Utilities
* System Libraries
* Kernel
* Hardware

## 🔑 Common Commands
- `ls`, `pwd`, `cd`, `chmod`, `ps`, `grep`, `tar -xvf`

---

## 3. AWS (Amazon Web Services)
**Definition**: AWS is Amazon’s cloud computing platform offering 200+ services like compute, storage, networking, and DevOps tools. It enables scalable, cost-efficient, and globally available infrastructure.

## ☁️ AWS Service Categories & Detailed Explanations

### Compute
* **Amazon EC2 (Elastic Compute Cloud):** Amazon EC2 provides scalable virtual servers in the cloud, allowing developers to configure security, networking, and storage capacity dynamically. It offers complete control over your computing resources and integrates smoothly with other AWS services to handle variable workloads effortlessly.
* **AWS Lambda:** AWS Lambda enables serverless compute, running backend code in response to events without managing underlying servers. It automatically scales your application by running code in response to triggers like HTTP requests or data modifications while you only pay for the exact compute time consumed.
* **Amazon ECS (Elastic Container Service):** Amazon ECS is a highly scalable, high-performance container orchestration service that supports Docker containers. It allows you to easily run and scale containerized applications across a managed cluster of Amazon EC2 instances or serverless infrastructure with Fargate.
* **Amazon EKS (Elastic Kubernetes Service):** Amazon EKS makes it easy to deploy, manage, and scale containerized applications using Kubernetes on AWS. It eliminates the need to install and operate your own Kubernetes control plane, ensuring high availability and seamless integration with the broader AWS ecosystem.
* **AWS Fargate:** AWS Fargate is a technology for Amazon ECS and EKS that allows you to run containers without having to manage servers or clusters. It removes the need to provision, configure, and scale groups of virtual machines, letting you focus solely on building and running your applications.
* **Auto Scaling:** Amazon EC2 Auto Scaling helps you maintain application availability and automatically add or remove EC2 capacity according to conditions you define. It ensures you have the correct number of Amazon EC2 instances running to handle the load of your application seamlessly.

### Storage
* **Amazon S3 (Simple Storage Service):** Amazon S3 delivers object storage designed for high durability, availability, and infinite scalability across various storage classes. It allows developers to securely store and protect any amount of data for a wide range of use cases, such as websites, mobile applications, and backup archives.
* **Amazon EBS (Elastic Block Store):** Amazon EBS offers block-level storage volumes for use with Amazon EC2 instances in the AWS Cloud. Each volume is automatically replicated within its Availability Zone to protect you from component failure, offering high performance and low-latency storage for databases and file systems.
* **Amazon EFS (Elastic File System):** Amazon EFS provides fully managed shared file storage using the Network File System (NFS) protocol for use with AWS cloud services and on-premises resources. It scales on-demand automatically from gigabytes to petabytes without needing provisioning, enabling applications to grow without disruption.
* **Amazon S3 Glacier:** Amazon S3 Glacier is a secure, durable, and extremely low-cost cloud storage class for data archiving and long-term backup. It provides flexible retrieval options ranging from a few minutes to hours, making it ideal for data that is infrequently accessed.
* **AWS Snowball:** AWS Snowball is a petabyte-scale data transport solution that uses secure devices to transfer large amounts of data into and out of the AWS Cloud. It provides a fast, secure, and cost-effective alternative to transferring massive datasets over high-cost networks.
* **AWS Storage Gateway:** AWS Storage Gateway is a hybrid cloud storage service that gives on-premises applications seamless access to virtually unlimited cloud storage. It combines on-premises software with cloud storage to provide secure integration between the company’s local environment and AWS.

### Databases
* **Amazon RDS (Relational Database Service):** Amazon RDS simplifies relational database administration for engines like MySQL, PostgreSQL, and SQL Server in the cloud. It manages routine database tasks such as patching, backup, recovery, and scaling, freeing up engineers to focus on application development.
* **Amazon Aurora:** Amazon Aurora brings enterprise-grade relational database performance with automated scaling, distributed storage, and high availability. It is MySQL and PostgreSQL-compatible, offering up to five times the throughput of standard MySQL at a fraction of the cost.
* **Amazon DynamoDB:** Amazon DynamoDB offers a fully managed NoSQL database service delivering seamless, single-digit millisecond latency at any scale. It features built-in security, continuous backups, automated multi-region replication, and in-memory caching for internet-scale applications.
* **Amazon Redshift:** Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud designed for fast query performance and analytics. It uses columnar storage technology to organize data and parallelize queries, allowing you to analyze massive datasets quickly.
* **Amazon ElastiCache:** Amazon ElastiCache is a fully managed in-memory data store and cache service that supports Redis and Memcached. It boosts web application performance by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.
* **Amazon Neptune:** Amazon Neptune is a fast, reliable, highly scalable graph database service designed to build and run applications that work with highly connected datasets. It helps developers easily query and navigate complex relationships between data points such as social networks and fraud detection systems.

### Networking
* **Amazon VPC (Virtual Private Cloud):** Amazon VPC gives users complete isolation and control over a logically isolated virtual network dedicated to their AWS environment. It enables you to launch AWS resources in a defined virtual network with customizable IP address ranges, subnets, route tables, and gateway configurations.
* **Amazon Route 53:** Amazon Route 53 acts as a highly scalable and available Domain Name System (DNS) web service to route end-user requests globally. It connects user requests to infrastructure running in AWS—such as EC2 instances and load balancers—while also managing domain registrations.
* **Amazon CloudFront:** Amazon CloudFront works as a high-speed Content Delivery Network (CDN) to securely cache and distribute content with low latency globally. It delivers data, videos, applications, and APIs to customers using a worldwide network of edge locations, accelerating performance.
* **ELB (Elastic Load Balancing):** Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses. It enhances the fault tolerance of your applications, providing the required high availability and seamless scaling.

### Security
* **AWS IAM (Identity and Access Management):** AWS IAM safely manages granular access permissions and identity federations across cloud resources. It allows you to securely control who can access your AWS services and resources by creating users, groups, and roles with specific permission policies.
* **AWS Cognito:** AWS Cognito provides simple and secure user authentication, authorization, and user management for web and mobile apps. It scales to millions of users and supports sign-in with social identity providers like Google, Facebook, and enterprise SAML identity providers.
* **AWS KMS (Key Management Service):** AWS KMS provides centralized, hardware-backed control over cryptographic keys used to encrypt application data across AWS services. It makes it easy to create and manage encryption keys while maintaining compliance with strict security regulations.
* **AWS Shield:** AWS Shield is a managed Distributed Denial of Service (DDoS) protection service that safeguards applications running on AWS. It provides always-on inline detection and automatic mitigations to minimize application downtime and latency.
* **AWS WAF (Web Application Firewall):** AWS WAF helps protect your web applications against common web exploits and bots that can affect availability or compromise security. It enables you to control traffic patterns by creating customized security rules based on conditions like IP addresses, HTTP headers, or body strings.
* **AWS GuardDuty:** AWS GuardDuty is a threat detection service that continuously monitors your AWS accounts and workloads for malicious activity and unauthorized behavior. It uses machine learning, anomaly detection, and integrated threat intelligence to identify potential security compromises.
* **AWS Inspector:** AWS Inspector is an automated vulnerability management service that continually scans your AWS workloads for software vulnerabilities and unintended network exposures. It provides a detailed security assessment report to help improve the security posture of your applications.

### DevOps Tools
* **AWS CloudFormation:** CloudFormation lets engineers model and provision infrastructure resources securely using human-readable templates written in YAML or JSON. It provides a common language for describing and provisioning all your infrastructure resources in your cloud environment.
* **AWS CodePipeline:** CodePipeline automates continuous integration and continuous delivery (CI/CD) workflows from code commit to production release. It builds, tests, and deploys your code every time there is a code change, based on the release workflow models you define.
* **AWS CodeBuild:** CodeBuild compiles source code, runs unit tests, and packages deployable software artifacts natively in a managed environment. It scales continuously and processes multiple builds concurrently, eliminating the need to provision and manage your own build servers.
* **AWS CodeDeploy:** CodeDeploy automates software deployments to various compute services such as Amazon EC2, AWS Fargate, Lambda, and on-premises servers. It makes it easier to release new features quickly, helps avoid downtime during deployment, and handles the updating of your applications.
* **AWS Elastic Beanstalk:** Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, and Docker. It handles the deployment details of capacity provisioning, load balancing, and automated scaling for you.

### Monitoring
* **Amazon CloudWatch:** Amazon CloudWatch monitors cloud resources and applications by collecting metrics, logs, and setting up automated system alarms. It gives you deep visibility into resource utilization, operational performance, and overall system health across your infrastructure.
* **AWS X-Ray:** AWS X-Ray tracks user requests through distributed microservices applications to help debug performance bottlenecks and errors. It provides an end-to-end view of requests as they travel through your application components, showing a visual map of the service architecture.
* **AWS Systems Manager:** Systems Manager gives operational insights and centralized control across hybrid cloud environments to manage configuration data. It allows you to view operational data from multiple AWS services and automate operational tasks across your EC2 and on-premises resource instances.

### Analytics & AI
* **Amazon Athena:** Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. It is serverless, so there is no infrastructure to setup or manage, and you only pay for the queries that you run.
* **AWS Glue:** AWS Glue is a serverless data integration service that makes it easy to discover, prepare, and combine data for analytics and machine learning. It automates the messy data preparation steps required before you can load data into data warehouses.
* **Amazon QuickSight:** Amazon QuickSight is a cloud-powered business intelligence service that makes it easy to deliver insights to everyone in your organization. It lets you build interactive dashboards, perform quick ad-hoc analysis, and get business insights from data easily.
* **Amazon SageMaker:** Amazon SageMaker enables developers and data scientists to build, train, and deploy machine learning models quickly at any scale. It provides every component needed for machine learning in a unified toolset to reduce the effort of bringing models to production.
* **Amazon Rekognition:** Amazon Rekognition makes it easy to add advanced computer vision to your applications for image and video analysis. It uses deep learning technology to identify objects, people, text, scenes, and activities, as well as detect any inappropriate content.
* **Amazon Lex:** Amazon Lex is a fully managed artificial intelligence service for building conversational interfaces into any application using voice and text. It brings deep learning functionalities of natural language understanding and automatic speech recognition to build engaging chatbots.

## 🔑 Key Terminology
- **Region** → Geographic area (e.g., ap-south-1 = Mumbai)
- **Availability Zone (AZ)** → Independent data centers in a region
- **Elasticity** → Scale resources up/down automatically
- **Scalability** → Handle growing workloads
- **High Availability (HA)** → Minimize downtime
- **Fault Tolerance** → Operate despite failures
- **ARN (Amazon Resource Name)** → Unique identifier for AWS resources

---

## 4. Git
**Definition**: Git is a distributed version control system that tracks code changes as snapshots. It allows branching, merging, and collaboration across teams with full project history stored locally.

## 🔄 Git Workflow
* Working Directory
* Staging Area - `git add`
* Local Repo - `git commit`
* Remote Repo - `git push`/`pull`

---

## 5. GitHub vs GitLab
**Definition**: GitHub is a cloud platform for hosting Git repositories with strong community collaboration. GitLab is a DevOps lifecycle platform that combines Git hosting with built-in CI/CD and deployment tools.

---

## 6. Docker
**Definition**: Docker is a containerization platform that packages applications and dependencies into portable containers. It ensures consistency across environments and supports microservices architecture.

## 🐳 Docker Workflow
* Code
* Dockerfile
* Build Image
* Run Container
* Deploy

---

## 7. Kubernetes (K8s)
**Definition**: Kubernetes (K8s) is an open-source container orchestration system. It automates deployment, scaling, and management of containerized applications across clusters of machines.

## ☸️ Kubernetes Architecture
* `kubectl apply`
* API Server
* etcd - store config
* Scheduler
* Assign Pod to Node
* Kubelet - run container
* Kube-proxy - networking
