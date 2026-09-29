# Day-1 : 🐳 Introduction to Containerization and Docker Basics ⚓


## Difference between Monolithic and Microservices

## Monolithic Architecture
- A single, unified application where all components are tightly coupled.
- Difficult to scale individual components.
- Deployment and updates require redeploying the entire application.
- Example: A traditional web application where frontend, backend, and database are all part of the same codebase.

## Microservices Architecture
- Application is broken down into smaller, independent services.
- Each service can be developed, deployed, and scaled independently.
- Enables better fault isolation and easier technology upgrades.
- Example: An e-commerce platform with separate services for user authentication, product catalog, and order management.

---

![Alt text](arch.gif)



# Difference between Traditional, Virtualization, and Containerization Deployment

| Deployment Type     | Description |
|--------------------|-------------|
| **Traditional**    | Directly installs applications on physical servers, leading to inefficient resource utilization. |
| **Virtualization** | Uses a hypervisor to create multiple virtual machines (VMs) on a single physical server. Each VM has its own OS. |
| **Containerization** | Uses container technology to package applications and dependencies together, sharing the host OS for lightweight and efficient deployment. |

---

# Introduction to Containerization 📦 
Containerization is a method of packaging applications and their dependencies together in isolated environments known as containers. It ensures that applications run consistently across different computing environments.

## Key Concepts:
- **Container**: A lightweight, standalone executable package that includes everything needed to run an application.
- **Image**: A template used to create containers. It includes application code, dependencies, and runtime.

---

# Introduction to Docker 🐳 
Docker is a platform for developing, shipping, and running applications in containers. It simplifies application deployment across different environments.

## Why Use Docker?
- Ensures consistent environments across development, testing, and production.
- Reduces infrastructure overhead and resource consumption.
- Improves application scalability and portability.

---

# Difference between Docker CE and Docker EE

| Feature         | Docker CE (Community Edition) | Docker EE (Enterprise Edition) |
|---------------|-----------------------------|-----------------------------|
| **License**   | Open-source and free        | Paid with enterprise support |
| **Security**  | Basic security features     | Advanced security features   |
| **Support**   | Community support           | Professional support from Docker |
| **Management** | Basic container management | Advanced container orchestration and management |

---

# Day-2 : Docker Container Commands

## Introduction
Docker is a containerization platform that allows developers to package applications and dependencies into lightweight, portable containers. This guide covers essential Docker container commands and hands-on experiments to help students understand container management.

---

## Essential Commands for Container Management using Docker

### 1. **Create a Container**
```sh
docker create --name my_container ubuntu
```
Creates a new container from an Ubuntu image but does not start it.

### 2. **Run a Container**
```sh
docker run -it ubuntu
```
Runs a new container interactively with an Ubuntu image.

### 3. **Run a Container in Detached Mode**
```sh
docker run -d --name my_container nginx
```
Starts a container in the background using the Nginx image.

### 4. **Stop a Running Container**
```sh
docker stop my_container
```
Stops the specified container.

### 5. **Start a Stopped Container**
```sh
docker start my_container
```
Starts an existing container.

### 6. **Remove a Container**
```sh
docker rm my_container
```
Removes a stopped container.

### 7. **List All Running Containers**
```sh
docker ps
```
Displays active containers.

### 8. **List All Containers (Running & Stopped)**
```sh
docker ps -a
```
Displays all containers, including stopped ones.



---

## Expose Applications to the World

### 1. **Run a Container and Expose a Port**
```sh
docker run -d -p 8080:80 nginx
```
Maps port `8080` on the host to port `80` inside the container.

### 2. **Run a Container with Environment Variables**
```sh
docker run -d -e "ENV_VAR=value" my_app
```
Passes environment variables to a container.

### 3. **Expose Random Ports Using `-P` Flag**
```sh
docker run -d -P nginx
```
Automatically assigns random ports to the container.

---

## Interacting with Containers using `exec`

### 1. **Execute Commands in a Running Container**
```sh
docker exec -it my_container bash
```
Attaches an interactive terminal to a running container.

### 2. **Copy Files from Host to Container**
```sh
docker cp my_file.txt my_container:/tmp/
```
Copies a file into a container.

### 3. **Copy Files from Container to Host**
```sh
docker cp my_container:/tmp/my_file.txt ./
```
Retrieves a file from a container.

---

## Inspect and Troubleshoot Containers

### 1. **Inspect a Container's Metadata**
```sh
docker inspect my_container
```
Retrieves detailed information about a container.

### 2. **View Logs from a Running Container**
```sh
docker logs my_container
```
Displays real-time logs from a container.

### 3. **Monitor Resource Usage of a Running Container**
```sh
docker stats
```
Shows CPU, memory, and network usage of running containers.

---

## Experiment with Docker Containers
### 1. **Create, Start, Stop, and Run Containers**
Follow the commands above to create and manage containers.

### 2. **Expose Applications to the World**
Use `-p` and `-P` flags to expose containerized applications to external networks.

### 3. **Interact with Containers using `exec`**
Try executing commands inside a running container using `docker exec`.

### 4. **Troubleshooting Steps**
- Use `docker ps -a` to check container status.
- Use `docker logs <container_name>` to view logs.
- Use `docker inspect <container_name>` to check metadata.

---

### docker rm -f $(docker ps -aq)

---
# Introduction to Docker Images and Its Naming

Docker images are lightweight, stand-alone, and executable software packages that include everything needed to run an application, including the code, runtime, libraries, environment variables, and dependencies.

## Naming Convention for Docker Images
A Docker image name follows this pattern:

```
<repository>/<image-name>:<tag>
```
### Example:
```
r123mahajan/new:latest
```
- **Repository**: Name of the repository (e.g., `nginx`, `ubuntu`)
- **Image Name**: A descriptive name for the image
- **Tag**: A label to identify different versions (e.g., `latest`, `v1.0`)

Example:
```
docker pull ubuntu:20.04
```

# Introduction to DockerHub and Amazon ECR

## DockerHub
Docker Hub is a cloud-based registry service that allows users to store and distribute container images.

- **Public and Private Repositories**: Users can create both public and private repositories.
- **Pre-built Images**: Official images such as `nginx`, `mysql`, and `ubuntu` are available.
- **User Authentication**: Requires login for pushing images to private repositories.

## Amazon ECR (Elastic Container Registry)
Amazon Elastic Container Registry (ECR) is a managed container image registry service provided by AWS.

- **Integration with AWS Services**: Works seamlessly with ECS and EKS.
- **Access Control**: Uses AWS IAM for authentication.
- **Private Registry**: Secure storage of images within AWS.

# Docker Image Commands

## 1. Pull an Image
Downloads an image from DockerHub or any registry.
```
docker pull <image-name>:<tag>
```
Example:
```
docker pull nginx:latest
```

## 2. Login to DockerHub
Authenticates the user with DockerHub.
```
docker login -u <username>

* example docker login -u r123mahajan
```
For Amazon ECR:
```
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```

## 3. Push an Image
Pushes an image to a remote repository (DockerHub or ECR).
```
docker push <repository>/<image-name>:<tag>

```
Example:
```
docker push myrepo/myapp:v1
```

## 4. Commit a Container as an Image
Creates a new image from a running container.
```
docker commit <container-id> <new-image-name>
```
Example:
```
docker commit abc123 my-custom-image:v1
```

## 5. Tag an Image
Tags an existing image with a new name or version.
```
docker tag <image-id> <repository>/<new-image-name>:<tag>
```
Example:
```
docker tag my-app:latest myrepo/my-app:v2
```

## 6. Remove an Image
Deletes an image from the local machine.
```
docker rmi <image-id>
```
Example:
```
docker rmi nginx:latest
```
###  Delete all Images
```
docker rmi -f $(docker images -aq)
```
# Understanding Docker Networking

## Introduction to Docker Network
Docker networking allows containers to communicate with each other, external services, and the host system. It provides flexibility in how containers interact with each other and the outside world.

## Different Network Drivers
Docker offers several network drivers, each suited for different use cases:

1. **Bridge Network (Default)**: 
   - Used when a container is started without specifying a network.
   - Containers can communicate with each other using container names.
   - Suitable for standalone applications.

   
2. **Host Network**: 
   - Removes network isolation between the container and the host.
   - The container shares the host's networking stack.
   - Best for performance-intensive applications where network isolation isn't needed.
   

3. **None Network**: 
   - The container has no network interfaces.
   - Useful for security-sensitive applications that do not require network access.
   

4. **Overlay Network**: 
   - Used in Docker Swarm mode for multi-host communication.
   - Connects containers across multiple Docker daemon instances.


   
5. **Macvlan Network**: 
   - Assigns a MAC address to a container, making it appear as a physical device.
   - Suitable for legacy applications requiring direct network access.
  
   
6. **IPvlan Network**: 
   - Similar to Macvlan but with more flexibility in IP management.
   - Offers better control over IP address assignment.

## Docker Network Commands
### Listing Networks
```sh
docker network ls
```
### Creating a Network
```sh
docker network create my_custom_network
```
### Inspecting a Network
```sh
docker network inspect my_custom_network
```
### Removing a Network
```sh
docker network rm my_custom_network
```
### Running a Container with a Custom Network
```sh
docker run -d --name my_container --network my_custom_network nginx
```
### Connecting an Existing Container to a Network
```sh
docker network connect my_custom_network my_container
```
### Disconnecting a Container from a Network
```sh
docker network disconnect my_custom_network my_container

```

2. **Run containers in the custom network:**
   ```bash
   sudo docker run --network my_custom_network --name container1 -d nginx

   sudo docker run --network my_custom_network --name container2 -d nginx

   docker network create --subnet "192.168.0.0/16" --driver bridge newnetwork
   
   docker run -d -P --network host nginx:latest
   ```
   
   # Day-5 : Docker Volumes

## Introduction to Docker Volume
Docker volumes are used for persisting data in Docker containers. Unlike bind mounts, volumes are managed by Docker and stored in a directory on the host machine that Docker manages (`/var/lib/docker/volumes/`). Volumes are the preferred way to handle data persistence in Docker because they are easier to manage and work across different environments.

### Why Use Docker Volumes?
- **Persistence:** Data remains even after the container is removed.
- **Ease of Management:** Docker manages volumes separately from containers.
- **Performance:** More efficient than bind mounts.
- **Backup & Restore:** Easier to move data between different environments.
- **Container Independence:** Volumes can be shared across multiple containers.

---
## Docker Volume Commands
### 1. Creating a Docker Volume
```sh
# Create a new volume
docker volume create my_volume
```
This creates a volume named `my_volume`.

### 2. Listing All Volumes
```sh
docker volume ls
```
This displays all the available Docker volumes.

### 3. Inspecting a Volume
```sh
docker volume inspect my_volume
```
This gives detailed information about the specified volume, including its mount path.

### 4. Removing a Volume
```sh
docker volume rm my_volume
```
This deletes the specified volume. (Ensure it is not being used by any container.)

### 5. Removing All Unused Volumes
```sh
docker volume prune
```
This removes all unused volumes to free up space.

---
## Using Docker Volumes with Containers
### 1. Creating a Volume and Attaching it to a Container
```sh
docker run -d --name my_container -v my_volume:/data nginx
```
This starts a container named `my_container`, mounts the `my_volume` to `/data` directory, and runs an `nginx` server.

### 2. Sharing a Volume Between Multiple Containers
```sh
docker run -d --name container1 -v shared_volume:/data nginx

docker run -d --name container2 --volumes-from container1 nginx
```
Both `container1` and `container2` share the same `shared_volume` for data persistence.

---
# Docker Volumes

When you create a Docker container, all the data inside the container is **temporary**.

If the container is deleted, all the data stored inside it is also deleted.

To solve this problem, Docker provides **Volumes**.

---

# What is a Docker Volume?

A **Docker Volume** is a special storage location managed by Docker that stores data **outside the container's writable layer**.

Because the data is stored separately:

- ✅ If the container is deleted, the data remains safe.
- ✅ A new container can reuse the same data.
- ✅ Multiple containers can share the same volume.
- ✅ Docker manages the storage location automatically.

### Simple Definition

> **A Docker Volume is persistent storage used to keep container data safe even after the container is removed.**

---

# Types of Persistent Storage in Docker

Docker provides two common ways to persist data:

1. **Bind Mount**
2. **Docker Volume**

---

# 1. Bind Mount

A **Bind Mount** maps a directory or file from the **host machine** directly into the container.

The data is stored on your local system, and Docker does not manage it.

### Command

```bash
sudo docker run -d \
  --name todaycdec \
  -P \
  -v /home/ubuntu/website:/usr/share/nginx/html \
  nginx
```

### Explanation

| Part | Description |
|------|-------------|
| `docker run` | Creates and starts a new container |
| `-d` | Runs the container in detached mode |
| `--name todaycdec` | Container name |
| `-P` | Publishes exposed ports automatically |
| `-v /home/ubuntu/website:/usr/share/nginx/html` | Mounts the host directory into the container |
| `nginx` | Uses the Nginx image |

### How it Works

```
Host Machine
┌───────────────────────────┐
│ /home/ubuntu/website      │
│   index.html              │
│   style.css               │
│   logo.png                │
└──────────────┬────────────┘
               │
               │ Bind Mount
               ▼
Container (Nginx)
┌───────────────────────────┐
│ /usr/share/nginx/html     │
│ index.html                │
│ style.css                 │
│ logo.png                  │
└───────────────────────────┘
```

### Advantages

- Easy to edit files from the host.
- Great for development.
- Changes on the host appear immediately inside the container.

### Disadvantages

- Depends on the host directory.
- Not portable across different machines.
- Docker does not manage the data.

---

# 2. Docker Volume

A **Docker Volume** is managed entirely by Docker.

Instead of using a host directory, Docker stores the data in its own storage location.

### Command

```bash
docker run -d \
  --name karan \
  -P \
  -v today:/usr/share/nginx/html \
  nginx
```

### Explanation

| Part | Description |
|------|-------------|
| `docker run` | Creates a container |
| `-d` | Detached mode |
| `--name karan` | Container name |
| `-P` | Publishes exposed ports |
| `-v today:/usr/share/nginx/html` | Mounts Docker Volume named **today** |
| `nginx` | Uses the Nginx image |

If the volume **today** does not exist, Docker automatically creates it.

---

## How it Works

```
Docker Managed Volume

today
┌──────────────────────────┐
│ index.html               │
│ style.css                │
│ images/                  │
└──────────────┬───────────┘
               │
               │ Mounted
               ▼
Container (Nginx)
┌───────────────────────────┐
│ /usr/share/nginx/html     │
│ index.html                │
│ style.css                 │
│ images/                   │
└───────────────────────────┘
```

---

# Where are Docker Volumes Stored?

On Linux:

```text
/var/lib/docker/volumes/
```

Example:

```text
/var/lib/docker/volumes/today/_data
```

Docker stores all files inside the `_data` directory.

---

# Check Existing Volumes

```bash
docker volume ls
```

Example Output

```text
DRIVER    VOLUME NAME
local     today
local     website
```

---

# Inspect a Volume

```bash
docker volume inspect today
```

---

# Remove a Volume

```bash
docker volume rm today
```

---

# Remove Unused Volumes

```bash
docker volume prune
```

---

# Bind Mount vs Docker Volume

| Feature | Bind Mount | Docker Volume |
|----------|------------|---------------|
| Managed By | Host OS | Docker |
| Storage Location | Any host directory | `/var/lib/docker/volumes` |
| Easy to Edit | ✅ Yes | ❌ Not directly |
| Portable | ❌ No | ✅ Yes |
| Best For | Development | Production |
| Data Persists | ✅ Yes | ✅ Yes |

---

# Real-Life Example

Suppose you are running an Nginx website.

### Using Bind Mount

```
Host Folder
/home/ubuntu/website

        │
        ▼

Container
/usr/share/nginx/html
```

- Edit `index.html` on the host.
- Refresh the browser.
- Changes appear immediately.

---

### Using Docker Volume

```
Docker Volume (today)

        │
        ▼

Container
/usr/share/nginx/html
```

- Docker stores the files.
- Even if the container is deleted, the website files remain.
- A new container can use the same volume.

---

# Key Points

- Docker containers are **ephemeral** (temporary).
- Volumes provide **persistent storage**.
- Bind Mounts use directories/files from the **host machine**.
- Docker Volumes are managed by **Docker**.
- Volumes survive container deletion.
- Multiple containers can share the same Docker Volume.
- Bind Mounts are ideal for **development**.
- Docker Volumes are recommended for **production** workloads.
# Day-6 : Introduction to Dockerfile
A **Dockerfile** is a script that contains a set of instructions to automate the process of building Docker images. It allows developers to create customized container images with pre-installed applications and dependencies.

## Key Dockerfile Instructions

### 1. `FROM` (Base Image)
Specifies the base image for the container.
```dockerfile
FROM ubuntu:latest
```

### 2. `LABEL` (Metadata)
Adds metadata like author or description.
```dockerfile
LABEL maintainer="Your Name <your.email@example.com>"
```

### 3. `RUN` (Execute Commands)
Executes shell commands inside the container.
```dockerfile
RUN apt update && apt install -y nginx
```

### 4. `CMD` (Default Command)
Defines the default command to run inside the container.
```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

### 5. `ENTRYPOINT` (Command with Arguments)
Defines a fixed command with arguments.
```dockerfile
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```

### 6. `ENV` (Environment Variables)
Defines environment variables.
```dockerfile
ENV APP_ENV=production
```

### 7. `ARG` (Build Arguments)
Defines arguments used during the build process.
```dockerfile
ARG APP_VERSION=1.0
```

### 8. `COPY` (Copy Files)
Copies files from the local system to the container.
```dockerfile
COPY index.html /usr/share/nginx/html/
```

### 9. `ADD` (Copy and Extract Files)
Similar to `COPY` but supports extracting compressed files.
```dockerfile
ADD archive.tar.gz /app/
```

### 10. `EXPOSE` (Expose Ports)
Informs Docker that the container will listen on the specified port.
```dockerfile
EXPOSE 80
```

### 11. `USER` (Specify User)
Sets the user for running commands inside the container.
```dockerfile
USER nginx
```

### 12. `WORKDIR` (Set Working Directory)
Sets the working directory inside the container.
```dockerfile
WORKDIR /app
```

## Building and Running a Docker Image

### Build a Docker Image
```sh
docker build -t my-image .
```

### Push Image to Docker Hub
```sh
docker login
docker tag my-image username/my-image
docker push username/my-image
```

### Pull an Image
```sh
docker pull username/my-image
```

### Run a Container from the Image
```sh
docker run -d -p 80:80 my-image
```
## Nginx Docker File
```
FROM ubuntu:latest
LABEL maintainer="sourabh"
RUN apt update && apt install -y nginx
COPY index.html /var/www/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
----

# Brief Introduction to Docker Compose

Docker Compose is a tool that allows you to define and run multi-container Docker applications using a simple YAML file. With Docker Compose, you can configure and launch your entire application stack, including services, networks, and volumes, in a single command.

## Key Features of Docker Compose:
1. **Single Configuration**: Define all services in a single `docker-compose.yml` file.
2. **Multi-Container Management**: Orchestrates multiple containers that form an application.
3. **Easy to Use**: Simple commands like `docker-compose up` and `docker-compose down` for deployment and teardown.
4. **Scalable**: Allows scaling individual services.

---

# Deploy a Three-Tier Application Using Docker Compose

In this example, we will deploy a **three-tier application** using Docker Compose. The three tiers include:
1. **Frontend**: Angular application (UI layer).
2. **Backend**: Spring Boot application (business logic layer).
3. **Database**: MySQL (data layer).

## Prerequisites:
- Install **Docker** and **Docker Compose** on your machine.
- Basic understanding of Docker and containers.

**Build and Start** the services using Docker Compose.
   ```bash
   docker-compose up --build
   ```
