#  📘 Kubernetes Notes #

## Introduction to Kubernetes
Kubernetes (often abbreviated as K8s) is an open-source container orchestration platform designed to automate the deployment, scaling, and management of containerized applications.
Initially developed by Google and now maintained by the Cloud Native Computing Foundation (CNCF), Kubernetes has become the industry standard for running containerized workloads at scale.

## Key Features:

- **Automated Scheduling:** Places containers on optimal nodes.
- **Self-Healing:** Restarts failed containers and replaces them automatically.
- **Horizontal Scaling:** Increases or decreases the number of replicas based on load.
- **Service Discovery & Load Balancing:** Exposes containers to the network and balances traffic.
- **Storage Orchestration:** Automatically mounts storage systems.

## Example:
Imagine you have a web application running in 10 containers. Instead of manually starting/stopping them, Kubernetes will:
- Deploy them evenly across multiple servers.
- Replace any crashed containers automatically.
- Route traffic to available containers without downtime.

## Why We Need an Orchestration Tool
When working with a small number of containers, manual management may seem simple. But in production environments with hundreds or thousands of containers, manual management becomes inefficient and error-prone.
Challenges Without Orchestration:

- **Scaling**: Manually starting/stopping containers based on demand.
- **Load Balancing**: Distributing traffic manually.
- **High Availability:** Restarting failed containers quickly.
- **Networking:** Connecting containers securely and consistently.
- **Updates:** Deploying new versions without downtime.

Example Problem:
If you run 100 containers across 5 servers and one server fails, you must:
- Detect the failure.
- Start replacement containers on another server.
- Update your load balancer.
An orchestration tool like Kubernetes does all of this automatically.

## Why Kubernetes?
Kubernetes has emerged as the preferred orchestration tool for several reasons:
1. **Open Source**: Vendor-neutral and community-driven.
2. **Scalability**: Designed to handle large-scale workloads.
3. **Portability**: Works across on-premises, cloud, and hybrid environments.
4. **Extensibility**: Highly customizable through APIs and plugins.
5. **Resilience**: Automatic healing of failed containers and rescheduling.
6. **Comprehensive Ecosystem**: Supported by a wide range of tools and platforms.

## Architecture of Kubernetes
Kubernetes follows a master-worker architecture.
### Control Plane (Master Components)
The control plane manages the cluster’s state.
- #### API Server (kube-apiserver):
Entry point for all cluster commands and communications.
Example: kubectl get pods talks to the API Server.
- #### Controller Manager (kube-controller-manager):
Watches the cluster’s state and ensures it matches the desired state.
Example: If a pod crashes, it tells the scheduler to start a new one.
- #### Scheduler (kube-scheduler):
Decides which worker node will run a new pod based on resources, policies, etc.
- #### etcd:
Key-value store holding the cluster's state and configuration.

###  Worker Node Components
- #### Kubelet:
Runs on every worker node; ensures pods are running correctly.
- #### Kube-Proxy:
Handles network rules and routing for pods.
- #### Container Runtime:
Runs containers (e.g., Docker, containerd, CRI-O).

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/bef9660a-6039-4a71-bf02-2fc9c27bca60" />


## **Kubernetes Architecture in a Nutshell**

- **Master Node**: The "manager" that makes decisions and keeps the cluster running.
- **Worker Node**: The "worker" that runs your applications.
- **Pods**: The "workers" of your apps, running one or more containers.
- **Additional Components**: Helpers like ConfigMaps, Secrets, Ingress, and Namespaces that make managing apps easier.

---

## **Example Workflow**

1. You tell Kubernetes to run an app using `kubectl`.
2. The **API Server** receives your request and forwards it to the **Scheduler**.
3. The **Scheduler** assigns the app to a **Worker Node**.
4. The **Kubelet** on the Worker Node starts the app in a **Pod**.
5. The **Controller Manager** ensures the app stays running, and **etcd** stores all the details.
6. If the app needs to talk to other apps, **Kube-proxy** handles the networking.

---


## Lifecycle of a Pod
A Pod is the smallest deployable unit in Kubernetes.
Pod Lifecycle Phases:
- **Pending** – Pod is accepted but not yet scheduled.
- **Running** – Containers are running on a node.
- **Succeeded** – Containers completed successfully.
- **Failed** – Containers terminated with an error.
- **Unknown** – Pod state can’t be determined.


## Cluster Creation Methods
Kubernetes clusters can be created in different ways depending on the environment:

| Tool/Service | Description                                              | Use Case                        |
| ------------ | -------------------------------------------------------- | ------------------------------- |
| **Minikube** | Runs Kubernetes locally inside a VM or Docker container. | Learning and local development. |
| **Kind**     | Runs Kubernetes inside Docker containers.                | CI/CD testing and quick setups. |
| **kubeadm**  | Bootstraps a Kubernetes cluster on your own machines.    | On-premises or cloud servers.   |
| **EKS**      | Managed Kubernetes by AWS.                               | Production workloads on AWS.    |
| **GKE**      | Managed Kubernetes by Google Cloud.                      | Production workloads on GCP.    |
| **AKS**      | Managed Kubernetes by Azure.                             | Production workloads on Azure.  |


## Introduction to Pods and Services

### Pods
A Pod is the smallest deployable unit in Kubernetes, encapsulating one or more containers with shared resources like storage and network.

- **Lifecycle**:
  - Pending → Running → Succeeded/Failed → Terminated

- **Use Cases**:
  - Running a single application container.
  - Running multiple containers that share resources and are tightly coupled (e.g., sidecar patterns).

### Services
Services provide stable networking and expose Pods to other applications or external traffic.

- **Types of Services**:
  1. **ClusterIP**: Exposes the service within the cluster.
  2. **NodePort**: Exposes the service on each node’s IP at a static port.
  3. **LoadBalancer**: Exposes the service to the internet using a cloud provider’s load balancer.

| Service Type     | Accessible From         | Typical Use Case                                | Example Access URL            |
| ---------------- | ----------------------- | ----------------------------------------------- | ----------------------------- |
| **ClusterIP**    | Inside cluster only     | Microservice-to-microservice communication      | `http://backend-service:8080` |
| **NodePort**     | External (via node IP)  | Testing or small setups without a load balancer | `http://<node-ip>:30080`      |
| **LoadBalancer** | Internet (via cloud LB) | Production applications accessible publicly     | `http://<public-ip>`          |

---

## Main Container and Sidecar Containers

### Main Container
The primary container that serves the main purpose of the application. Examples include application servers or web servers.

### Sidecar Container
An auxiliary container that provides supporting functionalities, such as logging, monitoring, or proxying.


## Run First Pod Using kubectl

1. **Create a Pod**:
   ```bash
   kubectl run nginx-pod --image=nginx --restart=Never
   ```
   - `--image`: Specifies the container image.
   - `--restart=Never`: Ensures the creation of a standalone Pod.

2. **Verify Pod**:
   ```bash
   kubectl get pods
   ```

3. **View Pod Details**:
   ```bash
   kubectl describe pod nginx-pod
   ```

---

## Expose Pod Using kubectl expose

1. **Expose Pod**:
   ```bash
   kubectl expose pod nginx-pod --type=NodePort --port=80
   ```
   - `--type=NodePort`: Exposes the Pod on a static port.
   - `--port`: Specifies the port the Pod listens on.

2. **Get Service Details**:
   ```bash
   kubectl get svc
   ```

3. **Access the Pod**:
   - Use the `<NodeIP>:<NodePort>` to access the exposed Pod.

---

## In-Depth: kubectl Usage

### Common Commands
- **View Resources**:
  ```bash
  kubectl get pods
  kubectl get svc
  kubectl get nodes
  ```

- **Delete Resources**:
  ```bash
  kubectl delete pod <pod-name>
  ```

- **Debugging**:
  ```bash
  kubectl logs <pod-name>
  kubectl exec -it <pod-name> -- /bin/bash
  ```

- Viewing Pod Events:
  ```bash
  kubectl describe pod <pod-name>
  ```

---
# Introduction to YAML Scripts and Kubernetes Manifest Files

## Understanding YAML Scripts
YAML (YAML Ain't Markup Language) is a human-readable data serialization format used extensively in Kubernetes for writing manifest files. It is used to define resources like Pods, Services, Deployments, and more.

### Key Features of YAML
- **Readable**: YAML is simple and easy to understand.
- **Indentation-Based**: Proper indentation is crucial.
- **Data Types**: Supports scalars (strings, numbers), lists, and dictionaries.

### Example YAML Structure
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

## Writing Manifest Files for Pods and Services
Kubernetes uses manifest files to describe the desired state of resources in the cluster. These files are written in YAML.

### Pod Manifest File
A Pod is the smallest deployable unit in Kubernetes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```
**Explanation**:
- `apiVersion`: The API version used (e.g., v1).
- `kind`: The type of resource (e.g., Pod).
- `metadata`: Metadata such as the name and labels.
- `spec`: Specification of the Pod, including containers and their properties.

### Service Manifest File
Services expose Pods to the network and enable communication between them.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-cl-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```
**Explanation**:
- `selector`: Matches the labels of the Pods to be exposed.
- `ports`: Defines the service port and the target port on the Pod.
- `type`: Specifies the service type (e.g., ClusterIP, NodePort)


---
## Kubernetes Networking: Intra-Pod and Inter-Pod Communication

Kubernetes networking is fundamental for ensuring smooth communication between various components, including pods, services, and external clients. It provides flexible networking configurations for intra-pod and inter-pod communication.

### Intra-Pod Communication
- **Definition**: Intra-pod communication refers to the communication between containers within the same pod.
- **Mechanism**: Containers in a pod share the same network namespace, which means they:
  - Share the same IP address.
  - Can communicate directly using `localhost` and exposed container ports.

### Inter-Pod Communication
- **Definition**: Inter-pod communication refers to the communication between pods.
- **Mechanism**:
  - Kubernetes assigns each pod a unique IP address.
  - Pods communicate directly using these IP addresses or via Kubernetes services.
  - Kubernetes ensures a flat network model where all pods can communicate without NAT.
 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
        - containerPort: 80

    - name: java
      image: openjdk:17
      command: ["tail", "-f", "/dev/null"]
      ports:
        - containerPort: 8080
      

    - name: mysql
      image: mariadb:latest
      env:
        - name: MYSQL_ROOT_PASSWORD    
          value: redhat
      ports:
        - containerPort: 3306
```
### Kubernetes Service Types for Networking

#### 1. **ClusterIP**
- Default service type.
- Exposes the service only within the cluster.
- Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clip-svc
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

#### 2. **NodePort**
- Exposes the service on a static port on each node.
- Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-np-svc
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 31195
```

#### 3. **LoadBalancer**
- Exposes the service externally using a cloud provider’s load balancer.
- Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-lb-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

## Key Networking Components

### 1. Pod IP
- Each pod gets a unique IP address within the cluster.
- Enables direct communication between pods without port conflicts.

### 2. Container Port
- The port exposed by the container inside the pod.
- Used for intra-pod communication.

### 3. Node IP
- IP address of the Kubernetes node.
- Used when accessing services exposed via NodePort or LoadBalancer.

### 4. Node Port
- A static port on the node that forwards traffic to the service.
- Example: Node IP + Node Port allows access to services from outside the cluster.

### 5. LoadBalancer
- Integrates with cloud provider load balancers to expose services externally.
- Automatically assigns external IPs for access.

## Examples

### Accessing a Pod Directly
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
- Accessing directly via Pod IP:
  - `curl <POD_IP>:80`

### Accessing a Service via NodePort
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-np-service
spec:
  type: NodePort
  selector:
    app: my-pod
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30001
```
- Access:
  - `http://<NODE_IP>:30001`

### Accessing a Service via LoadBalancer
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-lb-service
spec:
  type: LoadBalancer
  selector:
    app: my-pod
  ports:
  - port: 80
    targetPort: 80
```
- Access:
  - External IP provided by the load balancer.
  - `http://<EXTERNAL_IP>:80`

  ---
## ReplicationController and ReplicaSet
# 🚀 Kubernetes ReplicationController vs ReplicaSet

> 💡 **Goal:** Ensure the desired number of Pods are always running in the cluster.

---

# 📌 What is a ReplicationController (RC)?

> **Definition:**
>
> A **ReplicationController (RC)** is an older Kubernetes controller that ensures a specified number of Pod replicas are always running. If a Pod crashes or is deleted, RC automatically creates a new Pod to maintain the desired count.

### ✅ Features
- 📦 Maintains the desired number of Pods
- 🔄 Recreates Pods if they fail
- 📈 Supports scaling Pods up or down
- 🏷️ Supports **only equality-based label selectors**
- ❌ Deprecated (ReplicaSet is recommended)

---

## 🏗️ Architecture

```text
        👤 User
           │
           ▼
  ┌──────────────────┐
  │ ReplicationController │
  └──────────────────┘
           │
    ┌──────┼──────┐
    ▼      ▼      ▼
 🟢 Pod1 🟢 Pod2 🟢 Pod3
```

---

## ❌ Pod Failure

Suppose Pod2 crashes.

```text
Desired Pods = 3

🟢 Pod1
❌ Pod2
🟢 Pod3
```

ReplicationController detects:

```text
Desired = 3
Current = 2
```

Immediately creates a new Pod.

```text
🟢 Pod1
🟢 Pod3
🟢 Pod4 (New)
```

🎉 Desired state restored!

---

# 📝 ReplicationController YAML

```yaml
apiVersion: v1
kind: ReplicationController

metadata:
  name: nginx-rc

spec:
  replicas: 3

  selector:
    app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## ▶️ Create RC

```bash
kubectl apply -f rc.yaml
```

Check RC

```bash
kubectl get rc
```

Output

```text
NAME        DESIRED   CURRENT   READY
nginx-rc       3         3        3
```

Check Pods

```bash
kubectl get pods
```

Delete a Pod

```bash
kubectl delete pod <pod-name>
```

RC automatically creates another Pod.

---

# ⚠️ Limitation of ReplicationController

RC supports only **Equality-based Selectors**.

Example:

```yaml
selector:
  app: nginx
```

or

```yaml
selector:
  version: v1
```

❌ It **cannot** use advanced selectors like:

- app In (nginx, apache)
- version NotIn (v1)
- environment Exists

Because of this limitation, Kubernetes introduced **ReplicaSet**.

---

# 📌 What is a ReplicaSet (RS)?

> **Definition:**
>
> A **ReplicaSet** is the modern replacement for ReplicationController. It also ensures the desired number of Pods are always running but provides advanced label selectors, making it more flexible and powerful.

### ✅ Features

- 📦 Maintains desired number of Pods
- 🔄 Automatically recreates failed Pods
- 📈 Supports scaling
- 🏷️ Supports **MatchLabels**
- 🎯 Supports **MatchExpressions**
- 🚀 Used internally by Deployments
- ⭐ Recommended over ReplicationController

---

# 🏗️ Architecture

```text
            👤 User
               │
               ▼
      ┌────────────────┐
      │   ReplicaSet   │
      └────────────────┘
               │
      ┌────────┼────────┐
      ▼        ▼        ▼
   🟢 Pod1   🟢 Pod2   🟢 Pod3
```

---

## ❌ Pod Failure

```text
Desired Pods = 3

🟢 Pod1
❌ Pod2
🟢 Pod3
```

ReplicaSet detects

```text
Desired = 3
Current = 2
```

Creates

```text
🟢 Pod1
🟢 Pod3
🟢 Pod4 (New)
```

🎉 Cluster returns to the desired state.

---

# 📝 ReplicaSet YAML

```yaml
apiVersion: apps/v1
kind: ReplicaSet

metadata:
  name: nginx-rs

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## ▶️ Create ReplicaSet

```bash
kubectl apply -f rs.yaml
```

Check ReplicaSet

```bash
kubectl get rs
```

Output

```text
NAME         DESIRED   CURRENT
nginx-rs        3         3
```

---

# 🎯 Advanced Label Selectors

ReplicaSet supports **MatchExpressions**.

## 1️⃣ In Operator

```yaml
selector:
  matchExpressions:
  - key: app
    operator: In
    values:
    - nginx
    - apache
```

Meaning

```text
Manage Pods where

app = nginx
OR
app = apache
```

---

## 2️⃣ NotIn Operator

```yaml
selector:
  matchExpressions:
  - key: environment
    operator: NotIn
    values:
    - dev
```

Meaning

```text
Manage all Pods except

environment=dev
```

---

## 3️⃣ Exists Operator

```yaml
selector:
  matchExpressions:
  - key: environment
    operator: Exists
```

Meaning

```text
Manage every Pod that contains

environment=<any value>
```

---

## 4️⃣ DoesNotExist Operator

```yaml
selector:
  matchExpressions:
  - key: test
    operator: DoesNotExist
```

Meaning

```text
Manage Pods that do NOT have

test label
```

---

# 📈 Scaling Example

Current YAML

```yaml
replicas: 3
```

Change to

```yaml
replicas: 5
```

Apply again

```bash
kubectl apply -f rs.yaml
```

Result

```text
🟢 Pod1
🟢 Pod2
🟢 Pod3
🟢 Pod4
🟢 Pod5
```

---

Reduce

```yaml
replicas: 2
```

Result

```text
🟢 Pod1
🟢 Pod2
```

Extra Pods are deleted automatically.

---

# 🎯 Real Production Architecture

In production, we generally **do not create ReplicaSets directly**.

Instead:

```text
          👤 User
              │
              ▼
     ┌────────────────┐
     │  Deployment    │
     └────────────────┘
              │
              ▼
     ┌────────────────┐
     │   ReplicaSet   │
     └────────────────┘
              │
      ┌───────┼────────┐
      ▼       ▼        ▼
   🟢 Pod1 🟢 Pod2 🟢 Pod3
```

👉 Deployment automatically creates and manages the ReplicaSet.

---

# 🆚 ReplicationController vs ReplicaSet

| Feature | 🟡 ReplicationController | 🟢 ReplicaSet |
|----------|--------------------------|---------------|
| API Version | `v1` | `apps/v1` |
| Purpose | Maintain desired Pods | Maintain desired Pods |
| Auto-healing | ✅ | ✅ |
| Scaling | ✅ | ✅ |
| Equality-based Selector | ✅ | ✅ |
| MatchLabels | ❌ | ✅ |
| MatchExpressions | ❌ | ✅ |
| Used by Deployment | ❌ | ✅ |
| Recommended Today | ❌ No | ✅ Yes |

---

# 📝 Interview Questions

### ❓ What is a ReplicationController?

✅ It is an older Kubernetes controller that maintains the desired number of Pod replicas by automatically creating new Pods when existing Pods fail.

---

### ❓ What is a ReplicaSet?

✅ ReplicaSet is the next-generation version of ReplicationController. It performs the same job but supports advanced label selectors such as `matchLabels` and `matchExpressions`.

---

### ❓ What is the main difference?

| ReplicationController | ReplicaSet |
|------------------------|------------|
| Equality selectors only | Equality + Expression selectors |
| Deprecated | Recommended |
| Not used by Deployment | Used by Deployment |

---

# 💡 Easy Memory Trick

🟡 **ReplicationController**

> **Old Controller**
>
> "Keep Pods Running"

🟢 **ReplicaSet**

> **Modern Controller**
>
> "Keep Pods Running + Advanced Label Selection"

---

# 🎯 One-Line Summary

> 🚀 **ReplicationController = Old way to maintain Pods.**
>
> 🚀 **ReplicaSet = Modern way to maintain Pods with advanced label selectors.**
>
> ⭐ **In real-world projects, you usually create a Deployment, and the Deployment automatically creates and manages a ReplicaSet.**

# Deployments vs StatefulSets, Writing Manifests, and Understanding DaemonSets

## Introduction
In Kubernetes, different controllers manage specific workloads depending on the requirements of applications. Among the most commonly used are Deployments, StatefulSets, and DaemonSets. Each serves unique purposes in orchestrating containerized applications.

---

## Deployments vs StatefulSets

# 🚀 Deployment in Kubernetes

## 📌 What is a Deployment?

A **Deployment** is a Kubernetes object used to **deploy, manage, and update applications**.

A Deployment **does not create Pods directly**.

Instead, it creates a **ReplicaSet**, and the ReplicaSet creates and manages the Pods.

```text
             Deployment
                  │
                  ▼
             ReplicaSet
                  │
                  ▼
      Pod-1   Pod-2   Pod-3
```

### 💡 Relationship

- **Deployment** ➜ Manages ReplicaSet
- **ReplicaSet** ➜ Manages Pods
- **Pods** ➜ Run the application

---

# ❓ Why do we need Deployment if ReplicaSet already exists?

ReplicaSet has **only one responsibility**:

> **Maintain the desired number of Pods.**

For example, if we create a ReplicaSet with **3 replicas**:

```text
ReplicaSet

Pod-1
Pod-2
Pod-3
```

Suppose **Pod-2 crashes**.

```text
ReplicaSet

Pod-1
❌ Pod-2
Pod-3
```

ReplicaSet immediately creates a new Pod.

```text
ReplicaSet

Pod-1
Pod-2 (New)
Pod-3
```

✅ This feature is called **Self-Healing**.

So ReplicaSet is excellent at:

- Maintaining the desired number of Pods
- Recreating failed Pods

---

# 🤔 Then why was Deployment introduced?

Imagine your application is running with the following Docker image:

```text
Image = nginx:1.24
```

Now the development team releases a **new version**.

```text
Image = nginx:1.25
```

If you are using **only ReplicaSet**, updating the application becomes difficult.

You would have to:

- Create a new ReplicaSet manually.
- Delete the old ReplicaSet manually.
- Ensure users don't experience downtime.
- Manually roll back if the new version has bugs.

This process is complicated.

👉 To solve this problem, Kubernetes introduced **Deployment**.

Deployment automatically manages ReplicaSets and provides advanced deployment features.

---

# 📝 Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:1.24
```

Create the Deployment:

```bash
kubectl apply -f deployment.yaml
```

Deployment automatically creates:

```text
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pod-1
Pod-2
Pod-3
```

---

# 🔄 Updating the Application (Rolling Update)

Suppose the developer releases a new version.

Current image:

```yaml
image: nginx:1.24
```

Update it to:

```yaml
image: nginx:1.25
```

Apply the changes:

```bash
kubectl apply -f deployment.yaml
```

Deployment detects that the image has changed.

Instead of replacing everything at once, it creates a **new ReplicaSet**.

```text
                 Deployment
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
 ReplicaSet (Old)          ReplicaSet (New)
 Image: nginx:1.24         Image: nginx:1.25
```

Now Deployment performs a **Rolling Update**.

```text
Step 1

Old  Old  Old

↓

New  Old  Old

↓

New  New  Old

↓

New  New  New
```

✅ Users can continue accessing the application during the update.

This is why Deployments provide **Zero or Minimal Downtime**.

---

# ⏪ Rollback

Suppose version **nginx:1.25** contains a bug.

Instead of creating another ReplicaSet manually, simply run:

```bash
kubectl rollout undo deployment nginx-deployment
```

Deployment immediately switches back to the previous ReplicaSet.

```text
Before Rollback

ReplicaSet
Image: nginx:1.25

↓

Rollback

↓

ReplicaSet
Image: nginx:1.24
```

✅ Your application returns to the previous stable version within seconds.

---

# 📜 Rollout History

Deployment also keeps track of application versions.

View rollout history:

```bash
kubectl rollout history deployment nginx-deployment
```

Example output:

```text
REVISION   CHANGE-CAUSE
1          nginx:1.24
2          nginx:1.25
```

Using this history, Deployment knows exactly which version to roll back to.

---

# 📊 ReplicaSet vs Deployment

| Feature | ReplicaSet | Deployment |
|----------|------------|------------|
| Maintains desired Pods | ✅ Yes | ✅ Yes |
| Self-Healing | ✅ Yes | ✅ Yes |
| Scaling | ✅ Yes | ✅ Yes |
| Rolling Update | ❌ No | ✅ Yes |
| Rollback | ❌ No | ✅ Yes |
| Rollout History | ❌ No | ✅ Yes |
| Manages ReplicaSets | ❌ No | ✅ Yes |

---
```
# 🚀 StatefulSet in Kubernetes

## 📌 What is a StatefulSet?

A **StatefulSet** is a Kubernetes workload object used to deploy and manage **stateful applications**.

Unlike a Deployment, a StatefulSet provides:

- 🆔 Stable Pod Identity
- 💾 Persistent Storage
- 🌐 Stable Network Identity
- 🔄 Ordered Pod Creation, Scaling, Updates, and Deletion

It is mainly used for applications like **MySQL, MongoDB, PostgreSQL, Kafka, and ZooKeeper**.

---

# ❓ Why do we need StatefulSet?

A **Deployment** is designed for **stateless applications**.

If a Pod crashes:

```text
Deployment

Pod-a12
Pod-b34 ❌
Pod-c56

↓

New Pod

Pod-x78
```

The new Pod gets a **new name**, which is perfectly fine for web applications.

❌ But databases cannot work like this because they need:

- Same Pod Identity
- Same Hostname
- Same Storage

That's why Kubernetes provides **StatefulSet**.

---

# ✨ Features of StatefulSet

## 🆔 Stable Pod Identity

Each Pod gets a fixed name.

```text
mysql-0
mysql-1
mysql-2
```

If `mysql-1` crashes, Kubernetes recreates:

```text
mysql-1
```

✅ The Pod name never changes.

---

## 💾 Persistent Storage

Each Pod gets its own storage.

```text
mysql-0 → PVC-0
mysql-1 → PVC-1
mysql-2 → PVC-2
```

Even after restart, the Pod reconnects to its original storage.

---

## 🔄 Ordered Pod Creation

Deployment:

```text
Pod-1
Pod-2
Pod-3
```

✅ Created in parallel.

StatefulSet:

```text
mysql-0 ✅

↓

mysql-1 ✅

↓

mysql-2 ✅
```

Each Pod must become **Ready** before the next Pod is created.

---

## 🔄 Ordered Updates

When the image is updated:

```yaml
image: mysql:8.0
```

↓

```yaml
image: mysql:8.4
```

Pods are updated **one by one**.

```text
mysql-2

↓

mysql-1

↓

mysql-0
```

---

## 🗑️ Ordered Deletion

Pods are deleted in reverse order.

```text
mysql-2

↓

mysql-1

↓

mysql-0
```

---

# 📝 StatefulSet Manifest

```yaml
apiVersion: apps/v1
kind: StatefulSet

metadata:
  name: my-sts

spec:
  selector:
    matchLabels:
      app: my-app

  serviceName: "my-sts-service"

  replicas: 3

  template:
    metadata:
      labels:
        app: my-app

    spec:
      containers:
      - name: mysql
        image: mysql:latest

        env:
        - name: MYSQL_ROOT_PASSWORD
          value: redhat

        ports:
        - containerPort: 3306
```

Create the StatefulSet:

```bash
kubectl apply -f statefulset.yaml
```

Check StatefulSet:

```bash
kubectl get statefulset
```

Check Pods:

```bash
kubectl get pods
```

Output:

```text
my-sts-0
my-sts-1
my-sts-2
```

Notice that Pods have **fixed names**.

---

# 📊 Deployment vs StatefulSet

| Feature | 🚀 Deployment | 🗄️ StatefulSet |
|----------|---------------|----------------|
| Application Type | Stateless | Stateful |
| Pod Identity | Random | Stable |
| Pod Creation | Parallel | Sequential |
| Pod Updates | Parallel Rolling Update | Ordered Rolling Update |
| Pod Deletion | Any Order | Reverse Order |
| Storage | Ephemeral | Persistent |
| Pod Name | Random | Fixed (`my-sts-0`, `my-sts-1`) |

---

# 🌍 Common Use Cases

- 🗄️ MySQL
- 🍃 MongoDB
- 🐘 PostgreSQL
- 📩 Kafka
- 🦓 ZooKeeper
- 🔍 Elasticsearch
- 🗃️ Cassandra

StatefulSet is the best choice whenever an application requires **persistent data**, **stable Pod identity**, and **ordered Pod management**.
## Deployment Strategies

### Recreate Strategy
- Terminates all existing pods before creating new ones.
- Suitable for non-critical updates.

### Rolling Update Strategy
- Updates pods incrementally.
- Ensures minimal downtime and availability during updates.

### Canary Deployment
- Deploys a small subset of new pods alongside existing ones to test the update.

### Blue-Green Deployment
- Creates a new set of pods ("blue") while the old set ("green") remains active, enabling a smooth transition.

---

## Understanding DaemonSets

A **DaemonSet** ensures that a copy of a specific pod runs on all or selected nodes within a cluster. DaemonSets are typically used for system-level applications.

### Features of DaemonSets
- Runs one pod per node.
- Automatically schedules pods on newly added nodes.
- Ensures uninterrupted system monitoring and logging.

### Use Cases
- Log collection (e.g., Fluentd, Logstash).
- Monitoring (e.g., Prometheus Node Exporter).
- Network plugins (e.g., Calico, Weave).

### Manifest for a DaemonSet
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata: 
    name: my-dmnset
spec: 
    selector:
      matchLabels:
          app: fluentd
    template:
      metadata:
        labels:
          app: fluentd
      spec:
        nodeSelector:
            hostname: node01
        containers:
          - name: fluentd
            image: fluentd:latest
            ports:
              - containerPort: 24224
```
# ☸️ **Kubernetes ConfigMap & Secret – Complete Notes**

---

# 🗂️ **ConfigMap**

## 📖 **Definition**

A **ConfigMap** is a Kubernetes object used to store **non-sensitive configuration data** as **key-value pairs**.

Instead of hardcoding configuration inside a Docker image, we store it in a **ConfigMap** and inject it into Pods.

---

## 🎯 **Examples**

- 🌐 Database Hostname
- 🌍 Application Environment
- 🔗 API URL
- 🔢 Port Number
- 🚩 Feature Flags

> 💡 **Think of it as:**  
> A **ConfigMap** stores configuration that your application needs but is **not confidential**.

---

# ❓ **Why Do We Use ConfigMap?**

## ❌ **Without ConfigMap**

```text
Application
      │
      ▼
DB_HOST = mysql.company.com
PORT    = 3306
ENV     = Production
```

If any configuration changes, you must:

- ✏️ Edit source code
- 🐳 Build a new Docker image
- 📤 Push the image
- 🚀 Update Deployment

⚠️ This process is time-consuming.

---

## ✅ **With ConfigMap**

```text
Application
      │
      ▼
Reads Configuration
      │
      ▼
 ConfigMap
 ┌───────────────┐
 │ DB_HOST       │
 │ DB_PORT       │
 │ APP_ENV       │
 └───────────────┘
```

Now, if any configuration changes:

✅ Just update the **ConfigMap**

❌ No need to rebuild the Docker image.

---

# 🌍 **Real World Example**

Suppose your application connects to a MySQL database.

Instead of writing:

```text
DB_HOST=mysql
DB_PORT=3306
```

inside your application,

Store these values inside a ConfigMap:

```text
DB_HOST=mysql-service
DB_PORT=3306
```

Your application will read these values during runtime.

---

# 📄 **ConfigMap YAML**

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  DB_HOST: mysql-service
  DB_PORT: "3306"
  APP_ENV: production
```

---

# 🚀 **Create ConfigMap**

```bash
kubectl apply -f configmap.yaml
```

---

# 📋 **List ConfigMaps**

```bash
kubectl get configmap
```

---

# 🔍 **Describe ConfigMap**

```bash
kubectl describe configmap app-config
```

---

# 🌱 **Using ConfigMap as Environment Variables**

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 1

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx

        env:

        - name: DATABASE_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_HOST

        - name: DATABASE_PORT
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_PORT
```

Inside the container:

```text
DATABASE_HOST=mysql-service
DATABASE_PORT=3306
```

---

# 📦 **Using Entire ConfigMap**

Instead of defining every key separately:

```yaml
envFrom:
- configMapRef:
    name: app-config
```

✅ Every key inside the ConfigMap becomes an Environment Variable.

---

# 📂 **Using ConfigMap as Volume**

Instead of Environment Variables, ConfigMap can also be mounted as files.

```text
ConfigMap
   │
   ▼
config.txt
server.conf
   │
   ▼
Pod
   │
   ▼
/etc/config/
```

Example:

```yaml
volumes:
- name: config-volume
  configMap:
    name: app-config

containers:
- name: nginx
  image: nginx

  volumeMounts:
  - name: config-volume
    mountPath: /etc/config
```

---

# ✅ **Advantages of ConfigMap**

- 📦 Separate configuration from application
- 🚀 No need to rebuild Docker image
- 🔄 Easy to update
- ♻️ Reusable
- 📍 Centralized configuration

---

# 🔐 **Secret**

## 📖 **Definition**

A **Secret** is a Kubernetes object used to store **Sensitive Information** securely.

---

## 🔑 **Examples**

- 🔒 Passwords
- 🔑 API Keys
- 🎟️ Tokens
- 🖥️ SSH Keys
- 🔐 TLS Certificates
- 💾 Database Password

---

# ❓ **Why Not ConfigMap?**

ConfigMaps store values in **Plain Text**.

Anyone who has access to the cluster can read them.

Secrets are specifically designed to store confidential data.

---

# 🌍 **Real World Example**

Application needs:

```text
Username = admin
Password = Admin@123
```

❌ Don't write them directly inside Deployment.

Instead, store them inside a Secret.

```text
Secret

username
password
```

The application reads them securely.

---

# 🔡 **Base64 Encoding**

Encode Username

```bash
echo -n admin | base64
```

Output

```text
YWRtaW4=
```

Encode Password

```bash
echo -n Admin@123 | base64
```

Output

```text
QWRtaW5AMTIz
```

---

# 📄 **Secret YAML**

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: mysql-secret

type: Opaque

data:
  username: YWRtaW4=
  password: QWRtaW5AMTIz
```

---

# 🚀 **Create Secret**

```bash
kubectl apply -f secret.yaml
```

---

# 📋 **List Secrets**

```bash
kubectl get secret
```

---

# 🔍 **Describe Secret**

```bash
kubectl describe secret mysql-secret
```

---

# 🚀 **Complete Deployment Using `secretKeyRef`**

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-secret-demo

spec:
  replicas: 1

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest

        env:

        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: username

        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: password

        ports:
        - containerPort: 80
```

---

# 🔄 **How `secretKeyRef` Works**

```text
Secret
-------------------------
username = admin
password = Admin@123
-------------------------

        │
        ▼
secretKeyRef

        │
        ▼

Deployment

        │
        ▼

Pod

DB_USERNAME=admin
DB_PASSWORD=Admin@123
```

---

# 📦 **Using Entire Secret**

```yaml
envFrom:
- secretRef:
    name: mysql-secret
```

Every key becomes an Environment Variable automatically.

---

# 📂 **Using Secret as Volume**

```yaml
volumes:
- name: secret-volume
  secret:
    secretName: mysql-secret

containers:
- name: nginx
  image: nginx

  volumeMounts:
  - name: secret-volume
    mountPath: /etc/secret
```

Each key inside the Secret becomes a separate file.

---

# ⚖️ **ConfigMap vs Secret**

| Feature | 📦 ConfigMap | 🔐 Secret |
|----------|-------------|-----------|
| Stores | Non-sensitive Data | Sensitive Data |
| Examples | DB Host, URL, Port | Password, API Key |
| Encoding | Plain Text | Base64 Encoded |
| Security | Less Secure | More Secure |
| Mount | Env / Volume | Env / Volume |

---

# 🔄 **ConfigMap Flow**

```text
ConfigMap
     │
     ▼
Deployment
     │
     ▼
Pod
     │
     ▼
Application
```

---

# 🔄 **Secret Flow**

```text
Secret
     │
     ▼
Deployment
     │
     ▼
Pod
     │
     ▼
Application
```

---

# 🛠️ **Important Commands**

## 📦 ConfigMap

```bash
kubectl get configmap
kubectl describe configmap app-config
kubectl edit configmap app-config
kubectl delete configmap app-config

kubectl create configmap app-config \
--from-literal=DB_HOST=mysql-service \
--from-literal=DB_PORT=3306
```

---

## 🔐 Secret

```bash
kubectl get secret
kubectl describe secret
kubectl edit secret
kubectl delete secret

kubectl create secret generic mysql-secret \
--from-literal=username=admin \
--from-literal=password=Admin@123
```

---

# ⭐ **Best Practices**

## 📦 ConfigMap

- ✅ Store only non-sensitive data.
- ✅ Keep configuration separate from application code.
- ✅ Reuse ConfigMaps across Deployments.

---

## 🔐 Secret

- ✅ Never store passwords inside ConfigMaps.
- ✅ Use Secrets for Passwords, Tokens, API Keys, and Certificates.
- ✅ Enable Encryption at Rest in Production.
- ✅ Restrict access using RBAC.

---

# 🎤 **Interview Questions**

### ❓ What is ConfigMap?

A ConfigMap stores **non-sensitive configuration** as key-value pairs and provides them to Pods.

---

### ❓ What is Secret?

A Secret stores **sensitive data** such as passwords, API keys, tokens, and certificates.

---

### ❓ Difference between ConfigMap and Secret?

- 📦 ConfigMap → Non-sensitive Data
- 🔐 Secret → Sensitive Data

---

### ❓ How can ConfigMap or Secret be consumed?

- 🌱 Environment Variables (`env`)
- 📦 Entire Environment (`envFrom`)
- 📂 Mounted as a Volume

---

### ❓ Is Secret encrypted?

❌ No.

Secrets are **Base64 Encoded**, **not Encrypted** by default.

For Production, enable **Encryption at Rest**.

---

# 🧠 **Memory Trick**

📦 **ConfigMap = Configuration (Non-Secret)**

🔐 **Secret = Passwords, Tokens & Sensitive Data**```


## 1. Types of AutoScaling in Kubernetes

### 1.1. Horizontal Pod Autoscaler (HPA)
- **Purpose**: Adjusts the number of Pod replicas in a deployment based on CPU, memory, or custom metrics.
- **Use Case**: Scaling out/in to handle variable workloads.

### 1.2. Vertical Pod Autoscaler (VPA)
- **Purpose**: Adjusts the resource requests and limits (CPU/memory) of containers in a Pod.
- **Use Case**: Ensures optimal resource utilization for Pods.

### 1.3. Cluster Autoscaler
- **Purpose**: Adjusts the number of nodes in a cluster based on pending Pods that cannot be scheduled.
- **Use Case**: Dynamically increases or decreases cluster size.

---

## 2. Practical Steps: Horizontal Pod Autoscaler (HPA)

### Step 1: Prerequisites

-# 🚀 Metrics Server Installation (Minikube)

> **Purpose:** Metrics Server collects CPU and Memory usage of Pods and Nodes. HPA uses these metrics to automatically scale Pods.

---

# 📌 Step 1: Start Minikube

Start your Minikube cluster.

```bash
minikube start --driver=docker
```

Verify the cluster is running:

```bash
kubectl get nodes
```

**Expected Output:**

```text
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   5m    v1.35.x
```

---

# 📌 Step 2: Enable Metrics Server

Enable the Metrics Server addon.

```bash
minikube addons enable metrics-server
```

**Expected Output:**

```text
💡 metrics-server was successfully enabled
```

---

# 📌 Step 3: Verify the Metrics Server Pod

Check whether the Metrics Server Pod is running.

```bash
kubectl get pods -n kube-system
```

**Expected Output:**

```text
NAME                               READY   STATUS
metrics-server-xxxxxxxxxx          1/1     Running
```

> **Note:** If the Pod shows `0/1 Running`, wait for **30–60 seconds** and check again.

---

# 📌 Step 4: Verify Metrics

### Check Node Metrics

```bash
kubectl top nodes
```

**Example Output:**

```text
NAME       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
minikube   150m         8%     1450Mi          35%
```

---

### Check Pod Metrics

```bash
kubectl top pods -A
```

**Example Output:**

```text
NAMESPACE     NAME                             CPU(cores)   MEMORY(bytes)
kube-system   metrics-server-xxxx              4m           25Mi
default       nginx-deployment-xxxxx           2m           8Mi
```

---

# 
```


# ❗ Troubleshooting

If the Metrics Server Pod is still **0/1 Running**, restart the deployment:

```bash
kubectl rollout restart deployment metrics-server -n kube-system
```

Wait for **1 minute**, then verify again:

```bash
kubectl get pods -n kube-system
kubectl top nodes
```

---
```

### Step 2: 
 
# 🚀 Kubernetes Horizontal Pod Autoscaler (HPA) Practical

> **Prerequisite:** Metrics Server is installed and working (`kubectl top nodes` should display CPU and Memory usage).

---

# 📌 Step 1: Verify Metrics Server

Check whether the Metrics Server is working.

```bash
kubectl top nodes
```

### Example Output

```text
NAME        CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
minikube    120m         8%     1450Mi          35%
```

If this command works, you're ready to create an HPA.

---

# 📌 Step 2: Create Deployment

Create a file named **deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx

        ports:
        - containerPort: 80

        resources:
          requests:
            cpu: "100m"
            memory: "200Mi"

          limits:
            cpu: "200m"
            memory: "300Mi"
```

Apply the Deployment:

```bash
kubectl apply -f deployment.yaml
```

Verify:

```bash
kubectl get pods
```

### Expected Output

```text
NAME                                  READY   STATUS
nginx-deployment-xxxxx                1/1     Running
nginx-deployment-yyyyy                1/1     Running
```

---

# 📌 Step 3: Create Service

Create a file named **service.yaml**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service

spec:
  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80

  type: ClusterIP
```

Apply the Service:

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get svc
```

### Expected Output

```text
NAME            TYPE        CLUSTER-IP      PORT(S)
nginx-service   ClusterIP   10.xx.xx.xx     80/TCP
```

---

# 📌 Step 4: Create Horizontal Pod Autoscaler (HPA)

Create a file named **hpa.yaml**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: nginx-hpa

spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment

  minReplicas: 2
  maxReplicas: 5

  metrics:
  - type: Resource
    resource:
      name: cpu

      target:
        type: Utilization
        averageUtilization: 50
```

Apply the HPA:

```bash
kubectl apply -f hpa.yaml
```

Verify:

```bash
kubectl get hpa
```

### Example Output

```text
NAME        REFERENCE                        TARGETS   MINPODS   MAXPODS
nginx-hpa   Deployment/nginx-deployment      0%/50%       2         5
```

---

# 📌 Step 5: Watch HPA

### Open Terminal-2

Watch the HPA status:

```bash
kubectl get hpa -w
```

### Open Terminal-3

Watch Pods being created or deleted automatically:

```bash
kubectl get pods -w
```

---


```




- Simulate a load test to trigger scaling:
  ```bash
  kubectl run -i --tty load-generator --image=busybox --restart=Never --rm -- /bin/sh -c "while true; do wget -q -O- http://nginx-service; done"
  ```

- Observe the scaling behavior:
  ```bash
  kubectl get pods -w
  ```
# Kubernetes Ingress Notes

## 📌 1. Introduction to Ingress
- **Ingress** is an API object that manages **external access** to services in a Kubernetes cluster.
- Provides:
  - **HTTP/HTTPS routing**
  - **Path-based routing** (`/app1` → Service1, `/app2` → Service2)
  - **Host-based routing** (`app1.example.com` → Service1, `app2.example.com` → Service2)
- Ingress requires an **Ingress Controller** (commonly **NGINX Ingress Controller**).

---

## 📌 2. Install NGINX Ingress Controller (Manifests)
Apply the official manifest:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
**Verify installation:**

```sh
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```
### Sample Applications
✅ App1 (Deployment + Service)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app1
  template:
    metadata:
      labels:
        app: app1
    spec:
      containers:
      - name: app1
        image: hashicorp/http-echo
        args:
        - "-text=Hello from App1"
        ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: app1-svc
spec:
  selector:
    app: app1
  ports:
  - port: 80
    targetPort: 5678
```
### ✅ App2 (Deployment + Service)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app2
  template:
    metadata:
      labels:
        app: app2
    spec:
      containers:
      - name: app2
        image: hashicorp/http-echo
        args:
        - "-text=Hello from App2"
        ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: app2-svc
spec:
  selector:
    app: app2
  ports:
  - port: 80
    targetPort: 5678
```
### Ingress Examples
**✅ Path-based Routing**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: path-based-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-svc
            port:
              number: 80
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: app2-svc
            port:
              number: 80
```
**✅ Host/Name-based Routing**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: host-based-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: app1.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app1-svc
            port:
              number: 80
  - host: app2.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app2-svc
            port:
              number: 80
```
**to ckech annotations**
```sh
kubectl describe ing path-based-ingress
```
```sh
curl http://$(minikube ip)/app1
curl http://$(minikube ip)/app2
```
```sh
minikube addons enable ingress
```
```sh
curl http://$(minikube ip)/app1
```
