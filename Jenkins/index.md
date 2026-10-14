# Jenkins Notes

## 1. Core Definition
* **What it is:** The leading open-source automation server written in Java.
* **Primary Function:** Implements Continuous Integration (CI) and Continuous Delivery/Deployment (CD) workflows.
* **Main Goal:** Automates building, testing, and deploying code to reduce manual errors and accelerate release cycles.

## 2. Architecture
Jenkins uses a distributed Controller-Agent model to scale workloads:
* **Controller:** The central hub. It manages the Web UI, stores configurations, handles security, and orchestrates job scheduling.
* **Agents:** The workers. Executable units that receive instructions from the controller to compile code, run tests, and execute scripts. Can be static VMs or dynamic Docker/Kubernetes containers.

## 3. Key Concepts & Components
* **Job / Project:** A runnable task controlled by Jenkins (e.g., building a specific app).
* **Pipeline:** A user-defined model of a CD pipeline. It defines the entire build process (typically containing stages like Build, Test, and Deploy).
* **Jenkinsfile:** A text file that contains the definition of a Jenkins Pipeline and is checked into source control (Git). This is Pipeline as Code.
* **Plugins:** Modular extensions. Jenkins features over 2,300 plugins to integrate with tools like GitHub, AWS, Docker, Slack, and Kubernetes.

## 4. Types of Pipeline Syntax
* **Declarative Pipeline:** Simplest, structured, and strictly controlled format. Best practice for most modern setups.
* **Scripted Pipeline:** Follows an imperative syntax utilizing Groovy code. Highly flexible but harder to write and maintain.

## 5. Standard CI/CD Workflow
```text
[Developer] ──> Pushes Code ──> [Git/GitHub]
                              │ (Webhook Trigger)
                              ▼
[Production] <── Deploy <── Test <── [Jenkins Server]

```

## 6. Quick Code Example: Declarative Jenkinsfile

```groovy
pipeline {
    agent any // Run on any available agent
    stages {
        stage('Build') {
            steps {
                echo 'Compiling the application...'
                // Example: sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Running automated tests...'
                // Example: sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to staging environment...'
            }
        }
    }
}

```

```

```
