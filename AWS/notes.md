# Introduction to Virtualization and Cloud Computing

## Topics Covered

### 1. **Introduction to Virtualization**
- **What is Virtualization?**
  - Virtualization is the creation of a virtual version of hardware, allowing multiple operating systems to run on a single physical machine.
  
- **Types of Virtualization**
  - Server Virtualization: Multiple virtual servers on one physical machine.
  - Storage Virtualization: Combining multiple physical storage devices into a single virtual unit.
  - Network Virtualization: Abstracting network resources into a manageable virtual framework.

- **Key Benefits**:
  - Efficient resource utilization
  - Cost savings
  - Scalability
  - Simplified management

#### **Deep Dive Knowledge**:
- Virtualization uses a hypervisor (like VMware or VirtualBox) to manage VMs.
- Popular tools include KVM, Xen, and Microsoft Hyper-V.
- Real-time Example: A company can run a legacy Windows app on a Linux server using virtualization.

#### **Simple Explanation**:
- Think of virtualization as renting rooms in a house (the physical machine) where each room (VM) has its own tenant (OS).

### 2. **Virtualization vs Cloud**
- Virtualization is a foundational technology; cloud computing builds on it.
- **Comparison Table**:
  | **Feature**          | **Virtualization**                     | **Cloud Computing**                       |
  |-----------------------|----------------------------------------|-------------------------------------------|
  | **Definition**        | Technology to create virtual environments. | Delivery of on-demand computing resources via the internet. |
  | **Deployment**        | Local (on-premises servers).           | Hosted on the internet.                   |
  | **Scalability**       | Limited by physical hardware.          | Highly scalable without physical limits.  |
  | **Access**            | Internal network only.                | Accessible from anywhere with an internet connection. |

#### **Deep Dive Knowledge**:
- Virtualization focuses on optimizing hardware usage, while cloud emphasizes service delivery and flexibility.
- Real-time Example: Virtualization is like owning apartments in a building, whereas cloud is like Airbnb where anyone can rent as needed.

### 3. **Cloud Models (IaaS, PaaS, SaaS)**
- **Infrastructure as a Service (IaaS)**:
  - Virtualized hardware resources.
  - **Examples**: AWS EC2, Google Compute Engine.
  - **Real-time Example**: Hosting a website on AWS EC2 where you control the OS and applications.

- **Platform as a Service (PaaS)**:
  - Platforms for application development.
  - **Examples**: AWS Elastic Beanstalk, Google App Engine.
  - **Real-time Example**: Using AWS Elastic Beanstalk to deploy a web app without worrying about the underlying infrastructure.

- **Software as a Service (SaaS)**:
  - Software accessible over the internet.
  - **Examples**: Gmail, Salesforce.
  - **Real-time Example**: Using Google Docs to collaborate on documents in real-time.

#### **Simple Explanation**:
- **IaaS**: Like renting a raw apartment and furnishing it yourself.
- **PaaS**: Like renting a fully furnished apartment but adding your own decorations.
- **SaaS**: Like booking a hotel room where everything is ready.

### 4. **AWS Account Creation**

Follow these steps to create your AWS account:
1. Visit [AWS Free Tier](https://aws.amazon.com/free/).
2. **Sign Up**:
   - Enter your email address and password.
   - Create an account name.
3. **Account Type**: Choose "Personal" or "Professional."
4. **Payment Method**: Add a valid credit/debit card for identity verification.
5. **Verify Identity**: Enter your mobile number for OTP verification.
6. **Select Support Plan**: Choose "Basic (Free)."
7. Log in to the AWS Management Console.

#### **Deep Dive Knowledge**:
- AWS Free Tier offers 12 months of free services like EC2, S3, and Lambda with usage limits.
- Real-time Example: Use the free tier to deploy a simple static website using S3 and monitor costs via the AWS Billing Dashboard.

---

## Hands-On Practice
- **Objective**: Create an AWS account and explore the Management Console.
- **Tasks**:
  1. Sign up for an AWS account using the steps above.
  2. Navigate through the AWS Console to identify key services like EC2, S3, and RDS.

---

## Resources
- [AWS Free Tier Overview](https://aws.amazon.com/free/)
- [AWS Documentation](https://docs.aws.amazon.com/)
- [Virtualization Basics](https://www.vmware.com/topics/glossary/content/virtualization.html)

---

## Key Takeaways
- Virtualization is the backbone of cloud computing.
- Understand the differences between IaaS, PaaS, and SaaS.
- AWS provides a user-friendly interface for cloud services.

----

# Introduction to AWS Dashboard and EC2

## Topics Covered

### 1. **Introduction to AWS Dashboard**
The AWS Management Console is the primary interface for managing AWS resources. It is a web application that allows you to:
- Access and manage services like EC2, S3, RDS, and Lambda.
- Monitor your usage and billing.
- Set up security configurations and permissions.

#### **Key Components of the Dashboard**:
1. **Navigation Bar**: Displays your logged-in account, active region, and access to global services like billing and support.
2. **Service Search Bar**: Helps you quickly locate AWS services by name.
3. **Resource Groups**: Allows grouping and managing related resources.
4. **Recent Services**: A list of services you used recently for quick access.
5. **Build a Solution**: Quick links to common tasks such as launching an instance or creating a database.

#### **Best Practices**:
- Always verify the active region to avoid deploying resources in unintended locations.
- Use the Billing Dashboard to track costs and identify unusual usage patterns.

---

### 2. **Region vs Availability Zone (AZ)**
AWS resources are deployed in Regions and Availability Zones to ensure high availability, fault tolerance, and low latency.

#### **Region**:
- A geographical location, such as **US East (N. Virginia)** or **Asia Pacific (Mumbai)**.
- Each region is independent and contains multiple Availability Zones.
- Choose a region based on proximity to your users, compliance requirements, and cost considerations.

#### **Availability Zone (AZ)**:
- A physically distinct data center within a region.
- Each AZ is isolated but connected to other AZs in the same region via low-latency links.
- Example: The **Asia Pacific (Mumbai)** region has three AZs: ap-south-1a, ap-south-1b, and ap-south-1c.

#### **Why Regions and AZs Matter**:
- **High Availability**: Deploy resources across multiple AZs to ensure uptime during failures.
- **Low Latency**: Choose regions close to your users to minimize network delays.
- **Cost Optimization**: Some regions have lower costs for the same services.

---

### 3. **Introduction to EC2 Service**
Amazon Elastic Compute Cloud (EC2) is a core service for deploying scalable virtual servers in the cloud.

#### **Features**:
- **Scalability**: Easily scale instances up or down based on demand.
- **Customizable Instances**: Choose instance types based on CPU, memory, and storage needs.
- **Secure Access**: Use SSH keys and security groups for secure connections.
- **Flexible Pricing**:
  - **On-Demand Instances**: Pay per hour with no long-term commitments.
  - **Reserved Instances**: Commit for 1-3 years for lower prices.
  - **Spot Instances**: Bid for unused capacity at discounted rates.

#### **Use Cases**:
- Hosting web applications.
- Running batch jobs or machine learning models.
- Building development and testing environments.

---

### 4. **Creating Your First EC2 Instance (Ubuntu)**

#### **Steps to Launch an EC2 Instance**:
1. **Navigate to EC2 Service**:
   - Log in to the AWS Management Console.
   - Search for "EC2" in the service search bar and select it.

2. **Launch Instance**:
   - Click on the "Launch Instance" button.

3. **Choose an Amazon Machine Image (AMI)**:
   - Select **Ubuntu Server 20.04 LTS** (or another version as needed).

4. **Select Instance Type**:
   - Choose **t2.micro** for free tier eligibility (1 vCPU, 1 GB RAM).

5. **Configure Instance Details**:
   - Leave the default settings.
   - Optionally, enable Auto-Assign Public IP for internet access.

6. **Add Storage**:
   - Keep the default storage size (e.g., 8 GB).
   - Modify if additional storage is required.

7. **Add Tags**:
   - Create a tag to identify your instance (e.g., `Key: Name, Value: MyFirstInstance`).

8. **Configure Security Group**:
   - Create a new security group.
   - Add an inbound rule to allow **SSH (port 22)** from your IP address.

9. **Review and Launch**:
   - Verify your settings and click "Launch."
   - Select an existing key pair or create a new one for SSH access.

10. **Access Your Instance**:
    - Download the private key file (`.pem`) if creating a new key pair.
    - Use an SSH client (like PuTTY or Terminal) to connect:
      ```bash
      ssh -i /path/to/key.pem ubuntu@<Public-IP-Address>
      ```

#### **Best Practices**:
- Always restrict SSH access to specific IP addresses for security.
- Use Elastic IPs for instances requiring a static public IP.

---

## Resources
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Free Tier Details](https://aws.amazon.com/free/)
- [SSH Key Management](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)

---

## Key Takeaways
- The AWS Dashboard is your central hub for managing cloud resources.
- Regions and AZs provide resilience and flexibility for deployments.
- EC2 is a powerful service for launching virtual servers with customizable configurations.

----

# Security Groups and Instance Management

## Topics Covered

### 1. **Security Group (SSH and RDP Port)**
Security Groups act as virtual firewalls that control inbound and outbound traffic for your AWS instances. 

#### **Key Concepts**:
- **Inbound Rules**: Define the traffic allowed to reach your instance.
- **Outbound Rules**: Define the traffic allowed to leave your instance.
- Rules specify:
  - Protocol (e.g., TCP, UDP, ICMP).
  - Port Range (e.g., 22 for SSH, 3389 for RDP).
  - Source (IP address or another security group).

#### **Common Ports**:
- **SSH (Port 22)**: Used for secure remote access to Linux instances.
- **RDP (Port 3389)**: Used for remote access to Windows instances.

#### **Best Practices**:
- Restrict access to specific IP ranges for security.
- Use custom security groups for different applications or roles.

#### **Steps to Configure a Security Group**:
1. Open the EC2 Dashboard and navigate to "Security Groups."
2. Create a new security group.
3. Add inbound rules:
   - For SSH: Protocol = TCP, Port = 22, Source = My IP.
   - For RDP: Protocol = TCP, Port = 3389, Source = My IP.
4. Add outbound rules (default allows all traffic).
5. Attach the security group to your instance during or after creation.

---
# SSH Service: Secure Shell Basics

## What is SSH?
SSH (Secure Shell) is a cryptographic network protocol that provides a secure way to access remote servers and systems. It allows administrators and developers to log in, execute commands, and manage resources over an encrypted connection.

---

## Key Features of SSH
- **Secure Communication**: Uses encryption to secure the connection.
- **Authentication**: Supports various methods like password-based or key-based authentication.
- **Port Forwarding**: Allows secure tunneling of other protocols.
- **File Transfers**: Includes tools like `scp` and `rsync` for transferring files securely.
- **Session Management**: Offers features like persistent sessions and command execution.

---

## SSH in AWS
SSH is commonly used in AWS to manage EC2 instances and other Linux-based resources securely.

### Default Settings for AWS EC2:
- **Port**: 22 (must be open in the security group).
- **Authentication**: Key-pair (`.pem` file) generated during instance creation.

---

## Setting Up SSH for Remote Access

### 1. **Installing SSH Client**
Most operating systems come with SSH clients pre-installed. For example:
- **Linux/MacOS**: SSH is available by default in the terminal.
- **Windows**: Use `ssh` in PowerShell or install third-party tools like PuTTY.

### 2. **Connect to a Remote Server**
To connect to a remote server:
```bash
ssh -i /path/to/key.pem username@<remote-ip-address>
```
- Replace `/path/to/key.pem` with the path to your private key file.
- Replace `username` with the server's default user (e.g., `ubuntu` for Ubuntu instances).
- Replace `<remote-ip-address>` with the public IP of the remote server.

---

### 3. **Configuring SSH Access on AWS EC2**
1. **Verify Security Group**:
   - Ensure the inbound rule allows SSH (port 22).
   - Restrict access to your IP address for enhanced security.

2. **Set File Permissions**:
   ```bash
   chmod 400 /path/to/key.pem
   ```
   This ensures that the private key has the correct permissions.

3. **Connect Using the SSH Command**:
   ```bash
   ssh -i /path/to/key.pem ubuntu@<Public-IP>
   ```

4. **Troubleshooting Connection Issues**:
   - Verify that the instance is running.
   - Check the public IP and ensure it matches the instance.
   - Ensure your IP is allowed in the security group.

---

## Resources
- [Official OpenSSH Documentation](https://www.openssh.com/manual.html)
- [AWS EC2 Key Pair Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
- [Secure File Transfer with scp](https://linux.die.net/man/1/scp)

---

## Summary
SSH is an essential tool for securely managing remote servers. By understanding its features and applying best practices, you can ensure secure and efficient access to your resources. Let’s practice setting up and using SSH in our next session! 🚀


---
### 2. **Create Instance of Windows**
AWS EC2 supports launching instances with Windows operating systems for various use cases, such as hosting .NET applications or running Windows-based tools.

#### **Steps to Launch a Windows Instance**:
1. **Navigate to EC2**:
   - Log in to the AWS Management Console.
   - Search for "EC2" and open the service.

2. **Launch Instance**:
   - Click on "Launch Instance."

3. **Choose AMI**:
   - Select a Windows Server AMI, such as **Windows Server 2019 Base**.

4. **Choose Instance Type**:
   - Select **t2.micro** (free tier eligible).

5. **Configure Instance Details**:
   - Leave default settings.
   - Optionally, assign an Elastic IP for a static public IP.

6. **Add Storage**:
   - Keep the default storage size or increase based on your needs.

7. **Configure Security Group**:
   - Add an inbound rule for RDP (port 3389).

8. **Launch and Download Key Pair**:
   - Download the `.pem` key file for accessing the instance.

9. **Retrieve Password**:
   - After launching, go to the instance details.
   - Click on "Get Windows Password" and decrypt it using the `.pem` key file.

10. **Connect to the Instance**:
    - Use Remote Desktop Protocol (RDP) on your local machine:
      - Hostname: Public IP of the instance.
      - Username: Administrator.
      - Password: Retrieved from the AWS console.

---

### 3. **Remote Access of Linux and Windows Machines**

#### **Linux Instance Remote Access**:
1. **Prerequisites**:
   - Ensure SSH is allowed in the security group.
   - Have the `.pem` key file downloaded during instance creation.

2. **Using SSH Command**:
   ```bash
   ssh -i /path/to/key.pem ubuntu@<Public-IP>
   ```

3. **Troubleshooting**:
   - Verify that the instance is running.
   - Ensure your local machine's IP is added to the security group.

#### **Windows Instance Remote Access**:
1. **Enable RDP Access**:
   - Ensure RDP is allowed in the security group.

2. **Using Remote Desktop Client**:
   - Open Remote Desktop Connection (Windows) or install an RDP client (Linux/Mac).
   - Enter the public IP address of the instance.
   - Use the Administrator username and decrypted password.

3. **Troubleshooting**:
   - Check the RDP port in the security group.
   - Ensure the instance has a public IP.

---

## Resources
- [AWS Security Group Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)
- [Launching Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/LaunchingAndUsingInstances.html)
- [Connecting to Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

---

## Key Takeaways
- Security Groups are essential for controlling access to your instances.
- RDP and SSH are the primary protocols for remote access to Windows and Linux instances, respectively.
- Proper configuration of Security Groups ensures both accessibility and security.


----
# 


## 1. **Deploy Web Server Using NGINX**
NGINX is a lightweight, high-performance web server and reverse proxy server used to host websites and applications.

### **Steps to Deploy NGINX**:
1. **Update the Package Manager**:
   ```bash
   sudo apt update
   ```

2. **Install NGINX**:
   ```bash
   sudo apt install nginx -y
   ```

3. **Start and Enable the NGINX Service**:
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

4. **Verify the Installation**:
   - Open a web browser and navigate to the server's public IP.
   - You should see the default NGINX welcome page.

5. **Configure the Firewall** (If Enabled):
   - Allow HTTP and HTTPS traffic:
     ```bash
     sudo ufw allow 'Nginx Full'
     ```
   - Check the firewall status:
     ```bash
     sudo ufw status
     ```

6. **Host a Custom Website**:
   - Replace the default content:
     ```bash
     sudo nano /var/www/html/index.html
     ```
     Add your HTML content and save the file.

7. **Restart NGINX**:
   ```bash
   sudo systemctl restart nginx
   ```

8. **Verify the Website**:
   - Open the server's public IP in a web browser to view your hosted site.

---

## 2. **Deploy Web Server Using HTTPD**
HTTPD (Apache HTTP Server) is a widely-used open-source web server known for its flexibility and robustness.

### **Steps to Deploy HTTPD**:
1. **Update the Package Manager**:
   ```bash
   sudo yum update -y
   ```

2. **Install HTTPD**:
   ```bash
   sudo yum install httpd -y
   ```

3. **Start and Enable the HTTPD Service**:
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

4. **Verify the Installation**:
   - Open a web browser and navigate to the server's public IP.
   - You should see the default Apache HTTPD test page.

5. **Configure the Firewall**:
   - Allow HTTP and HTTPS traffic:
     ```bash
     sudo firewall-cmd --permanent --add-service=http
     sudo firewall-cmd --permanent --add-service=https
     sudo firewall-cmd --reload
     ```

6. **Host a Custom Website**:
   - Navigate to the default document root:
     ```bash
     cd /var/www/html
     ```
   - Replace or add content to `index.html`:
     ```bash
     sudo nano index.html
     ```
     Add your HTML content and save the file.

7. **Restart HTTPD**:
   ```bash
   sudo systemctl restart httpd
   ```

8. **Verify the Website**:
   - Open the server's public IP in a web browser to view your hosted site.

---

## Resources
- [NGINX Documentation](https://nginx.org/en/docs/)
- [Apache HTTPD Documentation](https://httpd.apache.org/docs/)
- [OpenSSH Documentation](https://www.openssh.com/manual.html)
- [Linux User Management Guide](https://linuxize.com/post/how-to-create-a-new-user-on-linux/)

---

## Key Takeaways
- NGINX and HTTPD provide robust platforms for hosting web applications.

------------

# Amazon EC2: Comprehensive Guide

This README provides detailed information about Amazon EC2, covering its dashboard, instance types, status checks, AMIs, launch templates, and purchasing options. The content is organized to ensure clarity and practical usability for users.

---

## **Amazon EC2 Dashboard Overview**

The Amazon EC2 Dashboard provides a centralized interface to manage EC2 resources. Key components of the dashboard include:

- **Instances**: View and manage running instances.
- **Launch Instances**: Create new EC2 instances using preconfigured settings.
- **Instance Types**: Explore the various types of instances available.
- **Elastic Block Store (EBS)**: Manage attached storage volumes.
- **Key Pairs**: Secure access to instances with SSH keys.
- **Security Groups**: Configure firewall settings for your instances.
- **Elastic IPs**: Allocate static public IP addresses to your instances.

---

## **Instance Types**

Amazon EC2 offers a wide range of instance types to suit diverse workloads. Below are the main categories:

### **General Purpose**
Balanced compute, memory, and storage.
- Examples: M5, T2, T3, M6a, Mac2

### **Compute Optimized**
Ideal for compute-intensive tasks.
- Examples: C5, C6i, C7g

### **Memory Optimized**
Designed for high-performance databases and memory-intensive applications.
- Examples: R5, X1, X2idn

### **Storage Optimized**
For workloads requiring high, sequential read/write access to large data sets.
- Examples: I3, D2, H1

### **Accelerated Computing**
Designed for applications requiring GPUs or hardware accelerators.
- Examples: P3, G5, Inf1

### **High-Performance Computing (HPC)**
For HPC applications requiring low latency and high performance.
- Examples: Hpc6a, Hpc7g

[Refer to AWS Instance Types Documentation](https://aws.amazon.com/ec2/instance-types/)

---

## **Status Checks for Amazon EC2 Instances**

### **Overview**
Amazon EC2 continuously monitors instance health via automated status checks. These checks ensure instances are running without issues.

- **System Status Check**: Verifies the infrastructure hosting your instance is functioning correctly.
- **Instance Status Check**: Monitors the software and network configuration of your instance.

### **Common Failure Causes**
- Networking issues
- Exhausted memory
- Corrupted file systems
- Kernel incompatibilities

### **CloudWatch Alarms**
You can set alarms to monitor status checks and take automated actions, such as instance recovery, if issues are detected.

[Refer to AWS Status Checks Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html)

---

## **AMI (Amazon Machine Image)**

### **AMI Types**
1. **EBS-backed AMI**: Boot volume stored on Amazon EBS.
2. **Instance-store-backed AMI**: Boot volume stored on local instance storage.

### **Creating an AMI**
1. Navigate to the EC2 dashboard.
2. Select the instance you wish to create an AMI from.
3. Choose **Actions > Image > Create Image**.
4. Provide the necessary details and click **Create Image**.

### **Copying an AMI**
You can copy AMIs between regions to enhance redundancy or deploy in multiple regions.

[Refer to AWS AMI Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)

---

## **Launch Templates**

Launch templates enable consistent configuration of EC2 instances. They support settings like:
- AMI ID
- Instance Type
- Security Groups
- Key Pair

### **Creating a Launch Template**
1. Open the EC2 dashboard.
2. Navigate to **Launch Templates**.
3. Click **Create Launch Template**.
4. Fill in required details, including AMI ID, instance type, and security groups.

[Refer to AWS Launch Template Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html)

---

# Purchasing Options

**On-Demand** - 
- Pay for what you use
- Linux or Windows - billing per second after the first minute
- All other OS - billing per hour
- It has the highest cost but no upfront payment
- No long-term commitment
- Recommended for short term and un-interrupted workloads where you can't predict the how application will behave 
- ideal for auto-scaling and unpredictable workloads

**Reserved Instances** - 
- Up to 72% discount compared to on-demand
- You reserve specific instance attributes (instance type, region, tenancy, os)
- Reservation period - 1 year(+discount) or 3 years(+++discount)
- Payment option - No upfront(+), partial upfront(++), all upfront(+++)
- Reserved Instances Scope - Regional or Zonal 
- Recommended for steady-state usage applications (eg. database)
- You can buy or sell in the reserved instance marketplace 
- Convertible Reserved Instance  - Can change the instance type, instance family, OS, scope, and tenancy. Up to 66% discount

**Spot Instances** - 
- Can get discounts up to 90% compared to on demand
- Instances that you can "lose" at any point of time if your max price is less that current spot price
- The most cost-efficient instances in AWS
- Useful for workloads that are resilient to failure - batch jobs, data analysis, image processing, any distributed workloads, workloads with a flexible start and end time
- not suitable for critical jobs or databases

**Dedicated Hosts** - 
- Physical servers dedicated just for your use
- You then have control over which instances are deployed on that host
- Available as on-demand or with a dedicated hosts reservation
- Useful if you have server-bound software licenses that use metrics like per-core, per-socket, per-VM
- Each dedicated host can run only on instance size and type
- Good for regulatory compliance or licensing requiremnets
- Predictable performance
- Complete isolation
- Most expensive option
- Billing is per host

**Dedicated Instances** - 
- Virtualized instances on hardware just for you
- Also uses physically dedicated EC2 servers
- Does not provide additional visibility and controls of dedicated host 
- Billing is per instance 
- May share hardware with other non-dedicated instances in the same account
- Available as on-demand, reserved instances, spot instances
- Cost additional $2 per hour per region

**Capacity Reservations** - 
- Reserve on-demand instances capacity in a specific AZ for any duration 
- You always have access to EC2 capacity when you need it 
- No time commitment (create/cancel anytime), no billing discount
- Combine with regional reserved instances and saving plans to benefit from billing discount
- You are charged at on-demand rate whether you run instances or not
- Suitable for short-term, uninterrupted workloads that need to be in a specific AZ

###**How to choose a purchasing option ???** - 

Let's understand these concepts of purchasing options by taking the example of booking a resort

- **On-demand** - coming and staying in a resort whenever we like, we pay the full price
- **Reserved instances** - like planning ahead and if we plan to stay for a long time, we may get a good discount
- **Spot instances** - the hotel allows people to bid for the empty room and the highest bidder keeps the rooms but can get kicked out anytime if anyone gives high price than them
- **Dedicated hosts** - we book an entire building of the resort 
- **Capacity reservations** - you book a room for a period with full price even if you don't stay


### **Overview**
Amazon EC2 offers several purchasing options to optimize costs:

1. **On-Demand Instances**: Pay for instances by the second.
2. **Savings Plans**: Commit to a consistent usage level for 1 or 3 years.
3. **Reserved Instances**: Commit to specific configurations for 1 or 3 years.
4. **Spot Instances**: Use spare EC2 capacity at reduced costs.
5. **Dedicated Hosts**: Full physical servers for your instances.
6. **Dedicated Instances**: Single-tenant instances.
7. **Capacity Reservations**: Reserve capacity in specific AZs.

[Refer to AWS Purchasing Options Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html)


----
## **EBS Volumes and Its Types**

Amazon Elastic Block Store (EBS) provides persistent block storage for use with Amazon EC2 instances. These volumes are highly available, reliable, and scalable.

### **Types of EBS Volumes**

1. **General Purpose SSD (gp3, gp2)**
   - Ideal for a wide range of workloads, including boot volumes, dev/test environments, and low-latency interactive apps.
   - **gp3** offers predictable IOPS and lower costs compared to **gp2**.

2. **Provisioned IOPS SSD (io1, io2)**
   - Designed for I/O-intensive applications, such as databases.
   - Offers high durability and supports features like Multi-Attach.

3. **Throughput Optimized HDD (st1)**
   - Suitable for frequently accessed, throughput-intensive workloads such as big data and log processing.

4. **Cold HDD (sc1)**
   - Ideal for infrequently accessed workloads, such as backups and archiving.

5. **Magnetic (Standard)**
   - Suitable for workloads where data is infrequently accessed.

[Refer to AWS EBS Volume Types Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html)

---

## **Attach Volumes, Create Partition, and Mount**

### **Attaching an EBS Volume to an EC2 Instance**
1. Go to the **EC2 Dashboard** on the AWS Management Console.
2. Select **Volumes** from the left menu.
3. Choose the volume you want to attach and click **Actions > Attach Volume**.
4. Select the instance to attach the volume to and click **Attach**.

### **Creating a Partition**
1. SSH into the EC2 instance to which the volume was attached.
2. Use the `lsblk` command to list all block devices and confirm the new volume is attached (e.g., `/dev/xvdf`).
3. Create a partition on the volume using the `fdisk` command:
   ```bash
   sudo fdisk /dev/xvdf
   ```
   - Press `n` to create a new partition.
   - Follow the prompts and press `w` to write changes.
4. Format the partition:
   ```bash
   sudo mkfs.ext4 /dev/xvdf1
   ```

### **Mounting the Volume**
1. Create a mount directory:
   ```bash
   sudo mkdir /mnt/ebs-volume
   ```
2. Mount the volume:
   ```bash
   sudo mount /dev/xvdf1 /mnt/ebs-volume
   ```
3. Verify the volume is mounted:
   ```bash
   df -h
   ```

### **Persisting the Mount (Optional)**
To make the mount persistent across reboots:
1. Open the `/etc/fstab` file:
   ```bash
   sudo nano /etc/fstab
   ```
2. Add the following entry:
   ```bash
   /dev/xvdf1   /mnt/ebs-volume   ext4   defaults,nofail   0   2
   ```
3. Save the file and exit.
4. Test the configuration:
   ```bash
   sudo mount -a
   ```

---

## **Summary**

- **EBS Volumes**: Provide persistent, reliable block storage.
- **Volume Types**: Include SSDs (gp3, gp2, io1, io2), HDDs (st1, sc1), and magnetic storage.
- **Operations**: Attach a volume, create partitions, format, and mount it to an EC2 instance.

----
## **Taking Backup Using Snapshots**

### **Manual Snapshot Creation**
1. Navigate to the **EC2 Dashboard** on the AWS Management Console.
2. Select **Volumes** from the left-hand menu.
3. Choose the volume to back up and click **Actions > Create Snapshot**.
4. Provide a description for the snapshot and click **Create Snapshot**.
5. To view snapshots, navigate to **Snapshots** in the EC2 Dashboard.


## **Automating Snapshots Using Snapshot Policies**

### **Create a Snapshot Lifecycle Policy**
1. Navigate to the **Amazon Data Lifecycle Manager (DLM)** in the AWS Management Console.
2. Click **Create lifecycle policy**.
3. Select **EBS Snapshot policy**.
4. Configure policy details:
   - Select resource type as **Volume**.
   - Add tags to identify the volumes to be included in the policy (e.g., `Environment:Production`).
5. Define a schedule:
   - Specify the frequency of snapshots (e.g., daily).
   - Define a retention period (e.g., retain snapshots for 7 days).
6. Review and create the policy.


## **Summary**

- **Partition Management**: Create and persist partitions using `fstab`.
- **Snapshots**: Manual and CLI-based snapshot creation.
- **Automation**: Implement lifecycle policies for scheduled backups.

---


# 📌 Introduction to NFS  

## 🔹 What is NFS?  
- **NFS = Network File System**  
- It is a way for computers to **share files over a network**.  
- Think of it like **Google Drive or OneDrive**, but for servers inside a network.  
- With NFS, you can create one storage location and let **multiple computers (clients)** connect and use it as if it were their own local disk.  

---

## 🔹 How does it work?  
- Two main parts:  
  1. **NFS Server** → The machine that has the actual storage and shares it.  
  2. **NFS Clients** → Other machines that connect to the server and access files.  

- Clients mount (attach) the shared folder and can **read/write files** just like a normal folder.  

---

## 🔹 Real-life Example  
- Imagine a company office:  
  - The **NFS Server** is like the office filing cabinet.  
  - Employees (clients) don’t keep their own copies, they just **open the same cabinet drawer** and work on the same files.  
  - If one person updates a document, everyone sees the update instantly.  

---

## 🔹 Key Features of NFS  
1. **Centralized Storage** – Files are stored in one place, not scattered across machines.  
2. **Shared Access** – Multiple servers can access the same data.  
3. **Scalability** – Add more clients without copying files everywhere.  
4. **Transparency** – To the client, it looks like a normal folder/directory.  
5. **Uses TCP/UDP Port 2049** – Default port for NFS communication.  

---

## 🔹 Why do we need NFS?  
- Without NFS: Every server would have its **own local files**, making it difficult to share or sync data.  
- With NFS: All servers point to the **same storage**, making collaboration and scaling easier.  

**Use Cases:**  
- Hosting a website on multiple servers → all servers can use **same images, code, or logs** from NFS.  
- In AWS → **EFS (Elastic File System)** is Amazon’s managed NFS service.  

---

## 🔹 Versions of NFS  
- **NFSv2** – Old version, basic sharing.  
- **NFSv3** – Supports large files, better performance.  
- **NFSv4** – Latest, more secure (supports encryption, ACLs).  
- AWS EFS uses **NFSv4.1** by default.  

---

# 📌 NFS in AWS Context  

### Is NFS Region-specific or AZ-specific?  

- **NFS (general protocol)** → Not tied to region/AZ, depends only on network reachability.  

- **Amazon EFS (managed NFS)**:  
  - **Region-specific** → An EFS file system exists only in one AWS region.  
  - **Multi-AZ** → Data is stored across multiple Availability Zones within that region.  
  - Instances in different AZs of the same region can mount the same EFS file system.  

---

## ✅ Quick Summary  
- **NFS protocol** → Not tied to region/AZ, just needs network connectivity.  
- **AWS EFS** →  
  - Region-specific.  
  - Multi-AZ within that region for high availability.  

👉 In AWS terms:  
**EFS is region-specific, but can be accessed from multiple AZs within that region.**

---

# 📌 Practical: Create and Mount Amazon EFS (NFS)

## 🔹 Step 1: Create EFS File System  
1. Open **Amazon EFS** in AWS Console.  
2. Click **Create file system**.  
3. Configure:  
   - **Name**: `my-efs`  
   - **VPC**: Select your VPC  
   - **AZs/Subnets**: Choose subnets (for high availability, select multiple AZs).  
4. **Security Groups**: Allow **NFS traffic (port 2049)**.  
5. Click **Create**.  

---

## 🔹 Step 2: Install NFS Utilities on EC2 Instances  

For **Amazon Linux**:  
```bash
sudo yum install -y amazon-efs-utils
```
# AWS EFS Practical Guide  

## 🔹 Step 1: Create an EFS File System  

1. Go to the **AWS Management Console → EFS**.  
2. Click **Create file system**.  
3. Select your **VPC** and Availability Zones (AZs).  
4. Attach **Mount Targets** in each AZ (linked to Subnets & Security Groups).  
5. Set **Performance mode** (General Purpose or Max I/O).  
6. Enable **Encryption** (recommended).  
7. Click **Create**.  

---

## 🔹 Step 2: Install NFS Utilities on EC2 Instances  

For **Ubuntu/Debian**:  
```bash
sudo apt-get update
sudo apt-get install -y nfs-common
```
**For Amazon Linux / RHEL / CentOS**
```sh
sudo yum install -y amazon-efs-utils
sudo yum install -y nfs-utils
```
**🔹 Step 3: Mount the EFS File System**

1.Get your EFS File System ID (e.g., fs-12345678) from the AWS console.
2.Create a mount point:
```sh
sudo mkdir -p /mnt/efs
```
**using NFS protocol**
```sh
sudo mount -t nfs4 -o nfsvers=4.1 fs-12345678.efs.us-east-1.amazonaws.com:/ /mnt/efs
```
**to mount**
```sh
systemctl daemon-reload
mount -a
```
**to verify**
```sh
df -h
```



# Basics of Networking

Networking is the backbone of modern computing and communication systems. It involves connecting devices to share resources, data, and information efficiently. Key components and concepts of networking include:

## Key Networking Components:
1. **IP Address (Internet Protocol Address):** A unique identifier assigned to each device on a network. It can be IPv4 (e.g., 192.168.1.1) or IPv6 (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334).

2. **Subnet Mask:** Defines the network and host portions of an IP address. For example, in IPv4, a subnet mask of 255.255.255.0 allows 256 total addresses, of which one is for the network and one for broadcast.

3. **Gateway:** A node that routes traffic from one network to another, typically connecting a private network to the internet.

4. **DNS (Domain Name System):** Translates human-readable domain names (e.g., www.example.com) into IP addresses.

5. **MAC Address (Media Access Control):** A hardware address that identifies devices within a local network.

## Types of Networks:
- **LAN (Local Area Network):** Small networks, typically within a single location, such as a home or office.
- **WAN (Wide Area Network):** Large networks spanning geographic locations, such as the internet.
- **VLAN (Virtual Local Area Network):** Logical segmentation of a LAN for better management and security.

## Networking Protocols:
1. **TCP/IP (Transmission Control Protocol/Internet Protocol):** Core protocol suite for communication over the internet.   #homework 
2. **HTTP/HTTPS:** Protocols for transferring hypertext data (webpages).
3. **FTP (File Transfer Protocol):** For transferring files over a network.
4. **SSH (Secure Shell):** For secure remote administration.

---

# CIDR (Classless Inter-Domain Routing)

## What is CIDR?
Classless Inter-Domain Routing (CIDR) is a method for efficiently allocating IP addresses and routing data. It replaces the older class-based IP addressing system (Class A, B, and C).

## CIDR Notation:
CIDR uses a suffix to specify the number of significant bits in the subnet mask. The format is:

```
IP Address/Prefix Length
```
For example:
- `192.168.1.0/24` means the first 24 bits are for the network, and the remaining 8 bits are for hosts.

## CIDR Example:
### Given `192.168.1.0/24`:
- **Subnet Mask:** 255.255.255.0
- **Total IPs:** 256 (2^8 for the last octet)
- **Usable IPs:** 254 (Subtract 1 for the network and 1 for broadcast address)
- **Range:** 192.168.1.1 to 192.168.1.254

## Benefits of CIDR:
1. **Efficient IP Address Allocation:** Prevents waste by allowing subnets of variable sizes.
2. **Improved Routing Efficiency:** Reduces the size of routing tables by grouping multiple networks under a single prefix.
3. **Scalability:** Supports hierarchical network design for better scalability.

## Practical Steps:

### Step 1: Understand Your Network Requirements
- Determine the number of hosts and subnets needed.
- Choose an IP range and prefix length that accommodates your needs.

### Step 2: Subnetting
- Use tools like subnet calculators or calculate manually.
- Example: Subnet `192.168.1.0/24` into smaller subnets like `192.168.1.0/26` and `192.168.1.64/26`.

### Step 3: Configure Networking Devices
- Assign IP addresses, subnet masks, and gateways to devices.
- Update DNS records as needed.

### Step 4: Verify Network Configuration
- Use commands like `ping`,  and `nslookup` to test connectivity and resolve issues.

---

---

# Introduction to VPC

## What is a VPC?
Amazon Virtual Private Cloud (VPC) allows you to launch AWS resources in a logically isolated network that you define. You have complete control over your virtual networking environment, including selecting your own IP address range, creating subnets, and configuring route tables and gateways.

### Key Features of a VPC:
- **Logical Isolation:** Operates within a region, providing control over network setup.
- **Subnets:** Divide your VPC into smaller segments based on your requirements.
- **Security:** Use security groups and network ACLs for fine-grained control.
- **Internet Gateway:** Attach to a VPC for internet access.
- **Private Connectivity:** Use VPN or Direct Connect to connect on-premises environments.
- **Elastic IPs:** Assign static IP addresses to resources in your VPC.

### VPC Types:
1. **Default VPC:** Automatically created by AWS in each region. It includes a public subnet in each Availability Zone, enabling immediate access to AWS services.
2. **Custom VPC:** Created manually to meet specific networking requirements, offering full control over the network configuration.

### Reserved IPs in AWS:
Within each subnet's CIDR block, AWS reserves the following IP addresses:
- **.0:** Network address.
- **.1:** Reserved by AWS for the VPC router.
- **.2:** Reserved for mapping Amazon-provided DNS.
- **.3:** Reserved for future use.
- **Last IP:** Reserved for the network broadcast address. 

For example, in a `/24` subnet (`192.168.1.0/24`):
- Reserved IPs: `192.168.1.0`, `192.168.1.1`, `192.168.1.2`, `192.168.1.3`, and `192.168.1.255`.

---

# CIDR Calculation for Subnets

## What is CIDR?
Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and routing. Instead of using the traditional class-based system, CIDR allows IP ranges to be split into subnets with flexible sizes.

### CIDR Notation:
A CIDR block is expressed as `IP address/prefix length`. For example, `192.168.0.0/24`:
- **192.168.0.0**: Network portion.
- **/24**: Indicates that the first 24 bits represent the network, leaving 8 bits for host addresses.

### Subnetting Calculation:
1. **Determine the Number of Hosts:**
   - Formula: `2^(32 - prefix length) - 2`
   - Example: `/24` = `2^(32 - 24) - 2 = 254 usable hosts`

2. **Divide a Larger Block:**
   - Example: To split `10.0.0.0/16` into four subnets, increase the prefix length to `/18`. Resulting subnets:
     - `10.0.0.0/18`
     - `10.0.64.0/18`
     - `10.0.128.0/18`
     - `10.0.192.0/18`

### Practical Example:
- **Base CIDR:** `10.0.0.0/16`
- **Required Subnets:** 4
- **Step:** Adjust prefix length to `/18` to accommodate each subnet.

---

# Create VPC and Subnet

## Step-by-Step Guide:

### Create a VPC:
1. Log in to the **AWS Management Console**.
2. Navigate to **VPC Dashboard**.
3. Click **Create VPC**.
4. Provide details:
   - **Name:** MyVPC
   - **IPv4 CIDR Block:** `10.0.0.0/16`
   - Enable DNS settings if required.
5. Click **Create VPC**.

### Create a Subnet:
1. Go to the **Subnets** section in the VPC Dashboard.
2. Click **Create Subnet**.
3. Select your VPC (e.g., `MyVPC`).
4. Specify details:
   - **Name:** PublicSubnet
   - **Availability Zone:** Choose an AZ (e.g., us-east-1a).
   - **CIDR Block:** `10.0.0.0/24`
5. Click **Create Subnet**.

### Verify:
1. Go to the **VPC Dashboard**.
2. Confirm that your VPC and Subnet appear in the list.
3. Associate your subnet with a route table if necessary.

---


## 1. Create an Internet Gateway (IGW) and Route

### What is an IGW?
An Internet Gateway is a horizontally scaled, highly available, and redundant VPC component that allows communication between instances in your VPC and the internet.

### Steps to Create an IGW and Attach to VPC:
1. **Navigate to the AWS Management Console:**
   - Go to the "VPC" section.

2. **Create an Internet Gateway:**
   - Select "Internet Gateways" from the left-hand menu.
   - Click "Create internet gateway."
   - Provide a name for the IGW (optional).
   - Click "Create internet gateway."

3. **Attach the IGW to a VPC:**
   - Select the IGW you just created.
   - Click "Actions" → "Attach to VPC."
   - Select the desired VPC from the dropdown list.
   - Click "Attach internet gateway."

### Create a Route Table and Associate with Subnets:
1. **Navigate to Route Tables:**
   - Under the "VPC" dashboard, click "Route Tables."

2. **Create a Route for Internet Access:**
   - Select the route table associated with the public subnet.
   - Click on the "Routes" tab and "Edit routes."
   - Add a route:
     - **Destination:** `0.0.0.0/0`
     - **Target:** Select the Internet Gateway ID.
   - Click "Save routes."

3. **Associate the Route Table with the Subnet:**
   - Go to the "Subnet Associations" tab.
   - Click "Edit subnet associations."
   - Select the public subnet.
   - Click "Save."

## 2. Launch Private and Public Instances

### Public Instance:
1. **Ensure Public Subnet Configuration:**
   - Verify that the subnet is associated with a route table pointing to an Internet Gateway.

2. **Launch an EC2 Instance:**
   - Navigate to the EC2 dashboard.
   - Click "Launch Instance."
   - Select an AMI (e.g., Amazon Linux 2).
   - Choose an instance type.
   - In the "Configure Instance" step, select the public subnet.
   - Enable "Auto-assign Public IP."
   - Add storage and tags as needed.
   - Configure a security group to allow SSH (port 22).
   - Launch the instance.

### Private Instance:
1. **Ensure Private Subnet Configuration:**
   - The subnet should not be associated with a route to an Internet Gateway.

2. **Launch an EC2 Instance:**
   - Follow the same steps as for a public instance.
   - In the "Configure Instance" step, select the private subnet.
   - Ensure "Auto-assign Public IP" is disabled.


## 3. NAT Gateway

### What is a NAT Gateway?
A Network Address Translation (NAT) Gateway enables instances in a private subnet to access the internet while preventing unsolicited inbound traffic from the internet.

### Steps to Create a NAT Gateway:
1. **Navigate to NAT Gateways:**
   - Under the "VPC" section, click "NAT Gateways."

2. **Create a NAT Gateway:**
   - Click "Create NAT gateway."
   - Select the public subnet.
   - Allocate an Elastic IP (EIP) for the NAT Gateway.
   - Click "Create NAT gateway."

3. **Update Route Table for Private Subnet:**
   - Go to "Route Tables."
   - Select the route table associated with the private subnet.
   - Click on the "Routes" tab and "Edit routes."
   - Add a route:
     - **Destination:** `0.0.0.0/0`
     - **Target:** Select the NAT Gateway ID.
   - Click "Save routes."


## 4. VPC Peering

### What is VPC Peering?
VPC Peering is a networking connection between two VPCs that enables you to route traffic between them using private IP addresses.

### Steps to Create a VPC Peering Connection:
1. **Create a Peering Connection:**
   - Navigate to the "VPC" dashboard.
   - Click "Peering Connections."
   - Click "Create peering connection."
   - Provide a name for the connection.
   - Select the requester VPC and the accepter VPC (can be in the same or different AWS accounts).
   - Click "Create peering connection."

2. **Accept the Peering Connection:**
   - If the accepter VPC is in the same account:
     - Select the peering connection.
     - Click "Actions" → "Accept request."
   - If in a different account, log in to the other account, navigate to "Peering Connections," and accept the request.

3. **Update Route Tables for Communication:**
   - Go to "Route Tables" in both VPCs.
   - Edit the routes to allow traffic to the CIDR block of the peer VPC.

4. **Update Security Groups:**
   - Ensure security group rules allow traffic between instances in the peered VPCs.

---

# Networking, Elastic IPs, Placement Groups, and Security

## NIC (Network Interface Controller) and Elastic IP

### NIC (Network Interface Controller)
A Network Interface Controller (NIC) in AWS is also referred to as an Elastic Network Interface (ENI). It is a virtual network interface that you can attach to an instance in a Virtual Private Cloud (VPC). ENIs provide network connectivity for instances and include attributes such as a private IPv4 address, an IPv6 address (optional), security groups, a MAC address, and a source/destination check flag.

#### Key Features of NIC:
- **Primary Network Interface**: Automatically created with every instance and cannot be detached.
- **Secondary Network Interfaces**: Can be attached or detached from instances, offering flexibility in multi-network configurations.
- **Custom Configuration**: Security groups, IP addresses, and MAC addresses can be customized.

#### Practical Steps to Manage ENIs:
1. Navigate to the **EC2 Dashboard** in the AWS Management Console.
2. Select **Network Interfaces** from the left-hand menu.
3. Create a new ENI by providing the VPC, Subnet, and security group details.
4. Attach the ENI to an instance from the **Actions** menu.

### Elastic IP
An Elastic IP (EIP) is a static IPv4 address designed for dynamic cloud computing. You can associate an EIP with your instance or ENI to allow external internet access.

#### Key Features of Elastic IP:
- **Static**: Remains unchanged unless manually released.
- **Reassignable**: Can be reassigned between instances in your account.
- **One Free IP**: AWS provides one Elastic IP per account without cost if it is associated with a running instance.

#### Practical Steps to Assign an Elastic IP:
1. Go to the **EC2 Dashboard** in the AWS Management Console.
2. Select **Elastic IPs** from the left-hand menu.
3. Click **Allocate Elastic IP address**.
4. Associate the EIP with an instance or network interface.

---

## Placement Groups
AWS Placement Groups are logical groupings of instances that allow applications to meet specific performance or redundancy requirements.

### Types of Placement Groups:
1. **Cluster Placement Group**:
   - Instances are placed close together within a single Availability Zone.
   - High throughput and low latency.
   - Ideal for HPC (High-Performance Computing) and big data workloads.

2. **Spread Placement Group**:
   - Instances are placed across different hardware within an Availability Zone.
   - Increases fault tolerance.
   - Ideal for small critical workloads.

3. **Partition Placement Group**:
   - Instances are divided into logical partitions.
   - Each partition is isolated from others.
   - Used for large distributed and replicated workloads such as HDFS, HBase, and Cassandra.

#### Practical Steps to Create a Placement Group:
1. Open the **EC2 Dashboard** in the AWS Management Console.
2. Select **Placement Groups** from the left-hand menu.
3. Click **Create Placement Group** and provide a name.
4. Choose the type of placement group and add the instances during or after creation.

---

## NACL (Network Access Control List) vs Security Group

### NACL (Network Access Control List):
- Acts as a firewall for controlling traffic in and out of one or more subnets.
- Operates at the subnet level.
- **Stateless**: Rules need to be explicitly defined for both inbound and outbound traffic.
- Supports rules by rule number with allow and deny actions.

### Security Group:
- Acts as a firewall for controlling traffic to and from an instance.
- Operates at the instance level.
- **Stateful**: Automatically allows responses to inbound traffic.
- Only supports allow rules.

### Key Differences:
| Feature                | NACL                         | Security Group             |
|------------------------|------------------------------|----------------------------|
| Scope                  | Subnet-level                 | Instance-level             |
| State                  | Stateless                    | Stateful                   |
| Rule Actions           | Allow and Deny               | Allow only                 |
| Default Behavior       | Allows all traffic by default| Denies all traffic by default|

#### Practical Steps to Manage NACL:
1. Navigate to the **VPC Dashboard** in the AWS Management Console.
2. Select **Network ACLs** from the left-hand menu.
3. Create a new NACL and associate it with subnets.
4. Add inbound and outbound rules as required.

#### Practical Steps to Manage Security Groups:
1. Open the **EC2 Dashboard**.
2. Select **Security Groups** from the left-hand menu.
3. Create a new security group by specifying rules for inbound and outbound traffic.
4. Attach the security group to instances during or after launch.


----
# Introduction to Load Balancer

A Load Balancer in AWS is a service that automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses. It acts as a "traffic cop," ensuring no single resource is overwhelmed, thereby improving application availability and reliability.

Load Balancers are designed to handle varying loads of application traffic while automatically scaling up or down based on demand. AWS offers Elastic Load Balancing (ELB), which supports three types of Load Balancers: Application Load Balancer, Network Load Balancer, and Gateway Load Balancer.

---

## Load Balancer and Its Types

AWS provides the following types of Load Balancers:

1. **Application Load Balancer (ALB):**
   - Operates at the **application layer** (Layer 7 of the OSI model).
   - Best suited for HTTP and HTTPS traffic.
   - Supports advanced request routing, based on URL, hostname, query string, or headers.
   - Features include WebSocket support, SSL termination, and integration with AWS Web Application Firewall (WAF).

2. **Network Load Balancer (NLB):**
   - Operates at the **transport layer** (Layer 4 of the OSI model).
   - Designed for TCP, UDP, and TLS traffic.
   - Best for high-performance use cases that require extremely low latency.
   - Provides static IP addresses and preserves the source IP of the client.

3. **Gateway Load Balancer (GWLB):**
   - Designed to deploy, scale, and manage third-party virtual appliances such as firewalls, intrusion detection, and prevention systems.
   - Operates at Layer 3 (network layer).

---

## Difference: Application Load Balancer vs. Network Load Balancer

| Feature                     | Application Load Balancer (ALB)           | Network Load Balancer (NLB)           |
|-----------------------------|-------------------------------------------|---------------------------------------|
| **OSI Layer**              | Layer 7 (Application Layer)               | Layer 4 (Transport Layer)            |
| **Traffic Type**           | HTTP, HTTPS                               | TCP, UDP, TLS                        |
| **Routing**                | Content-based (URL, headers, etc.)        | Connection-based                     |
| **Performance**            | Optimized for web applications            | High throughput and low latency      |
| **Source IP Preservation** | Not preserved (uses Load Balancer IP)     | Preserved                            |
| **Use Case**               | Web applications, microservices           | Gaming, real-time communication      |
| **Static IP Support**      | No                                        | Yes                                  |

---

## Practical Steps

### Step 1: Create a Load Balancer
1. **Navigate to the EC2 Dashboard:**
   - Log in to the AWS Management Console.
   - Go to **Services** > **EC2** > **Load Balancers**.

2. **Select Load Balancer Type:**
   - Choose **Application Load Balancer** or **Network Load Balancer** depending on your use case.

3. **Configure Load Balancer Settings:**
   - Provide a name for the Load Balancer.
   - Select the **Scheme** (Internet-facing or internal).
   - Choose **IP address type** (IPv4 or dualstack).

4. **Configure Listeners:**
   - Define listener ports and protocols (e.g., HTTP:80, HTTPS:443).

5. **Configure Target Group:**
   - Specify the target type (Instance, IP, or Lambda).
   - Provide health check settings to ensure targets are healthy.

6. **Review and Create:**
   - Review the settings and click **Create Load Balancer**.

### Step 2: Attach Targets
- Register your EC2 instances or other resources with the Target Group.
- Ensure that your instances are in the same VPC as the Load Balancer.

### Step 3: Test Load Balancer
- Access the DNS name of the Load Balancer from your browser or use tools like `curl` to test the setup.

---

Let me know if you need more details or assistance with implementation!

------

# Auto Scaling and its Types

Auto Scaling in AWS helps maintain application availability by automatically adjusting capacity to ensure stable and predictable performance. Auto Scaling dynamically scales the number of EC2 instances based on demand.

## Types of Auto Scaling:

1. **Dynamic Scaling**
   - Automatically adjusts the number of instances based on demand.
   - Uses scaling policies like Target Tracking, Simple Scaling, and Step Scaling.

2. **Scheduled Scaling**
   - Allows you to scale at specific times based on predictable load changes.
   - Useful for applications with known peak hours.

3. **Predictive Scaling**
   - Uses machine learning to forecast future traffic and scales resources proactively.
   - Reduces latency and ensures application availability.

---

# Create Launch Template

A Launch Template simplifies the process of configuring and launching EC2 instances. Follow these steps:

1. **Navigate to EC2 Console:**
   - Log in to AWS Management Console.
   - Go to the EC2 Dashboard.

2. **Create Launch Template:**
   - Select **Launch Templates** from the left menu.
   - Click **Create Launch Template**.

3. **Configure the Template:**
   - **Launch Template Name:** Provide a descriptive name.
   - **Template Version Description:** Optional but useful for versioning.
   - **AMI ID:** Choose an Amazon Machine Image.
   - **Instance Type:** Select the instance type (e.g., t2.micro).
   - **Key Pair:** Choose an existing key pair or create a new one.
   - **Network Settings:** Configure security groups and VPC settings.
   - **Storage:** Specify root volume size and additional storage if needed.

4. **Review and Create:**
   - Review the details.
   - Click **Create Launch Template**.

---

# Create Auto Scaling Group (Demo)

1. **Navigate to Auto Scaling Groups:**
   - In the EC2 Dashboard, select **Auto Scaling Groups**.

2. **Create Auto Scaling Group:**
   - Click **Create Auto Scaling Group**.

3. **Basic Configuration:**
   - **Group Name:** Enter a descriptive name.
   - **Launch Template:** Select the previously created Launch Template.

4. **Network Settings:**
   - Select a VPC and subnets where the instances will launch.

5. **Set Scaling Policies:**
   - Choose scaling policies like Target Tracking, Step Scaling, or Simple Scaling.
   - Optionally configure notifications and tags.

6. **Review and Create:**
   - Review all settings and click **Create Auto Scaling Group**.

---

# Demo Auto Scaling using Stress Command

1. **Launch Instances:**
   - Ensure Auto Scaling is set up with dynamic scaling policies.

2. **Install Stress Tool:**
   - SSH into an instance.
   - Install the stress tool:
     ```bash
     sudo yum install -y stress
     ```

3. **Simulate Load:**
   - Run the following command to simulate high CPU usage:
     ```bash
     stress --cpu 2 --timeout 300
     ```
   - Monitor the Auto Scaling Group as it adjusts the number of instances.

---

# Scheduled Scaling Theory

Scheduled Scaling enables pre-planned scaling adjustments based on known traffic patterns. For example:

- Scale up to 10 instances every day at 8 AM.
- Scale down to 2 instances at 10 PM.

**Steps to Configure:**
- Navigate to Auto Scaling Group settings.
- Add a scheduled action.
- Define the time and desired capacity.

---

# Predictive Scaling Theory

Predictive Scaling uses machine learning to forecast traffic and scales resources in advance. It analyzes historical data to:

- Anticipate usage trends.
- Provision resources proactively.

**Advantages:**
- Reduced latency.
- Optimized cost by avoiding over-provisioning.

Predictive Scaling can be configured through AWS Auto Scaling policies with minimal manual intervention.

---

With these configurations, you can ensure that your application remains scalable and cost-efficient while meeting user demands effectively.

----
## 1. Introduction to IAM Services

### What is IAM?
AWS Identity and Access Management (IAM) allows you to manage access to AWS services and resources securely. IAM helps control who can use your resources (authentication) and what actions they can perform (authorization).

### Key Features:
- Centralized control of AWS resources.
- Secure access to AWS services.
- Granular permissions for users and groups.
- Multi-Factor Authentication (MFA).
- Integration with third-party identity providers.

---

## 2. Creating IAM Users

### Steps:
1. **Navigate to the IAM Console:**
   - Log in to the AWS Management Console.
   - Go to **Services** > **IAM**.

2. **Create a User:**
   - Click **Users** > **Add Users**.
   - Provide a unique **User Name**.
   - Select **Access Type** (e.g., Console Access, Programmatic Access).

3. **Set Permissions:**
   - Attach existing policies directly.
   - Add the user to a group with appropriate permissions.

4. **Review and Create:**
   - Review details and click **Create User**.

---

## 3. Assigning Console and Programmatic Access

### Types of Access:
1. **Console Access:**
   - For web-based AWS Management Console access.
   - Requires a password setup.

2. **Programmatic Access:**
   - For AWS CLI, SDKs, or APIs.
   - Provides an **Access Key ID** and **Secret Access Key**.

### Steps to Assign Access:
- While creating a user, select the desired access type.
- Ensure policies are attached to grant required permissions.

---

## 4. Group Creation and Policy Attachment

### Steps:
1. **Create a Group:**
   - Navigate to **Groups** > **Create New Group**.
   - Provide a descriptive group name.

2. **Attach Policies:**
   - Select existing AWS-managed policies or create a custom policy.

3. **Add Users to Group:**
   - Select users to add to the group for inheriting its permissions.

---

## 5. Practical Demo: Policy Implementation and Restriction Tests

### Objective:
- Implement policies to manage access control.
- Test restrictions to validate policies.

### Steps:
1. **Create a Custom Policy:**
   - Navigate to **Policies** > **Create Policy**.
   - Use the visual editor or JSON editor to define permissions.

2. **Attach Policy:**
   - Attach the policy to users or groups.

3. **Test Policy Restrictions:**
   - Log in as the user with restricted access.
   - Attempt actions outside the policy scope to verify restrictions.

---

## 6. Different Types of Roles, Importance of Roles & Practical Demo for Role

### Types of Roles:
1. **Service Role:**
   - Used by AWS services to perform actions on your behalf.

2. **Cross-Account Role:**
   - Grants access to resources in another AWS account.

3. **IAM Role for EC2:**
   - Allows EC2 instances to interact with AWS services securely.

### Importance of Roles:
- Enables temporary access without sharing credentials.
- Ensures secure and flexible access control.
- Facilitates automation and service-to-service interactions.

### Practical Demo:
1. **Create a Role:**
   - Navigate to **Roles** > **Create Role**.
   - Select the type (e.g., AWS Service, Another AWS Account).

2. **Attach Policies:**
   - Choose policies to define permissions for the role.

3. **Assign Role to a Service:**
   - For example, attach the role to an EC2 instance during or after launch.

4. **Test Role Functionality:**
   - Access the AWS service using the assigned role.
   - Validate permissions by performing actions defined in the role's policies.

------
# Introduction to AWS CLI

AWS Command Line Interface (CLI) is a powerful tool that enables users to interact with AWS services using commands. It helps automate tasks and manage AWS services efficiently.

---

## AWS CLI Setup and Configuration

### Step 1: Install AWS CLI
1. Download the AWS CLI installer for your operating system.
2. Follow the installation instructions provided [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

### Step 2: Configure AWS CLI
Run the following command to configure AWS CLI:
```bash
aws configure
```
Provide the following details:
- Access Key ID
- Secret Access Key
- Default Region
- Output Format (e.g., JSON, table, text)

### Step 3: Verify Installation
Run this command to verify:
```bash
aws --version
```

---

## AWS CLI Commands for S3 Management

### 1. List S3 Buckets
```bash
aws s3 ls
```

### 2. Create a Bucket
```bash
aws s3 mb s3://<bucket-name>
```

### 3. Upload a File to S3
```bash
aws s3 cp <file-path> s3://<bucket-name>/
```

### 4. List Objects in a Bucket
```bash
aws s3 ls s3://<bucket-name>/
```

### 5. Delete an Object
```bash
aws s3 rm s3://<bucket-name>/<object-key>
```

### 6. Sync Local Folder to S3
```bash
aws s3 sync <local-folder-path> s3://<bucket-name>/
```
### 87. empty  bucket to S3
```
aws s3 rm s3://cbz14buck  --recursive
```
 ### 8. remove empty to S3
 ```
 
aws s3api delete-bucket --bucket cbz14buck  --region  ap-south-1
```
---

## AWS CLI Commands for EC2 Management

### 1. List EC2 Instances
```bash
aws ec2 describe-instances
```

### 2. Start an EC2 Instance
```bash
aws ec2 start-instances --instance-ids <instance-id>
```

### 3. Stop an EC2 Instance
```bash
aws ec2 stop-instances --instance-ids <instance-id>
```

### 4. Terminate an EC2 Instance
```bash
aws ec2 terminate-instances --instance-ids <instance-id>
```

### 5. Create a Key Pair
```bash
aws ec2 create-key-pair --key-name <key-name>
```

### 6. Create a Security Group
```bash
aws ec2 create-security-group --group-name <group-name> --description "<description>"
```

### 7. Add Inbound Rule to Security Group
```bash
aws ec2 authorize-security-group-ingress --group-id <group-id> --protocol tcp --port 22 --cidr 0.0.0.0/0
```

---

## AWS CLI Commands for IAM Management

### 1. List IAM Users
```bash
aws iam list-users
```

### 2. Create an IAM User
```bash
aws iam create-user --user-name <user-name>
```

### 3. Attach a Policy to an IAM User
```bash
aws iam attach-user-policy --user-name <user-name> --policy-arn <policy-arn>
```

### 4. Create an IAM Group
```bash
aws iam create-group --group-name <group-name>
```

### 5. Add a User to a Group
```bash
aws iam add-user-to-group --user-name <user-name> --group-name <group-name>
```

### 6. Create a Custom IAM Policy
```bash
aws iam create-policy --policy-name <policy-name> --policy-document file://<policy-document.json>
```

---

## AWS CLI Commands for VPC Management

### 1. List VPCs
```bash
aws ec2 describe-vpcs
```

### 2. Create a VPC
```bash
aws ec2 create-vpc --cidr-block <CIDR-block>
```

### 3. Create a Subnet
```bash
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block <CIDR-block>
```

### 4. Create an Internet Gateway
```bash
aws ec2 create-internet-gateway
```

### 5. Attach Internet Gateway to VPC
```bash
aws ec2 attach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <igw-id>
```

### 6. Create a Route Table
```bash
aws ec2 create-route-table --vpc-id <vpc-id>
```

### 7. Add Route to Route Table
```bash
aws ec2 create-route --route-table-id <route-table-id> --destination-cidr-block 0.0.0.0/0 --gateway-id <igw-id>
```

### 8. Associate Route Table with Subnet
```bash
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <route-table-id>
```

---
.

## Refer:https://docs.aws.amazon.com/cli/latest/

These hands-on tasks will help reinforce the understanding and practical usage of AWS CLI commands.

-------

# Monitoring and Logging with CloudWatch

Amazon CloudWatch is a monitoring and observability service provided by AWS. It provides actionable insights to optimize resource utilization, application performance, and operational health. Below are the key aspects of CloudWatch:

---

## **CloudWatch Dashboard**

### Overview:
The CloudWatch Dashboard is a customizable interface that allows you to visualize and monitor AWS resources and applications in a single view.

### Steps to Create a Dashboard:
1. **Navigate to CloudWatch Console:**
   - Go to the AWS Management Console and open the CloudWatch service.
2. **Create Dashboard:**
   - Click on **Dashboards** from the left-hand menu.
   - Select **Create Dashboard**.
3. **Add Widgets:**
   - Choose the type of widget (e.g., Line, Number, Stacked Area).
   - Select the resource and metrics to monitor (e.g., EC2 CPU utilization, S3 Bucket Requests).
4. **Save Dashboard:**
   - Provide a name and save the dashboard for future use.

### Benefits:
- Real-time monitoring.
- Single view for multiple AWS services.
- Easy sharing and collaboration.

---

## **Metrics**

### Overview:
Metrics are the fundamental monitoring data in CloudWatch. Each AWS service sends metrics to CloudWatch, which you can analyze and use for alarms.

### Common Metrics:
- **EC2 Instances:** CPUUtilization, DiskReadOps, NetworkIn.
- **S3 Buckets:** NumberOfObjects, BucketSizeBytes.
- **RDS Databases:** CPUUtilization, FreeStorageSpace.

### Steps to View Metrics:
1. **Navigate to Metrics:**
   - In the CloudWatch Console, click on **Metrics** in the left-hand menu.
2. **Select a Namespace:**
   - Choose a namespace (e.g., EC2, S3, Lambda).
3. **Filter and Analyze:**
   - Use filters and search to locate specific metrics.
   - View graphs and statistics for the selected metric.

### Use Cases:
- Identify performance bottlenecks.
- Track resource utilization trends.

---

## **Alarms**

### Overview:
CloudWatch Alarms help you respond to changes in your metrics by sending notifications or performing actions like scaling.

### Steps to Create an Alarm:
1. **Navigate to Alarms:**
   - In the CloudWatch Console, click on **Alarms** from the left-hand menu.
2. **Create Alarm:**
   - Click on **Create Alarm**.
3. **Select a Metric:**
   - Choose the metric to monitor.
4. **Set Thresholds:**
   - Define the condition (e.g., CPU utilization > 80%).
5. **Configure Actions:**
   - Choose actions like sending an Amazon SNS notification or triggering an Auto Scaling action.
6. **Review and Create:**
   - Verify the configuration and create the alarm.

### Use Cases:
- Alert when an EC2 instance is over-utilized.
- Automatically scale resources when thresholds are breached.

---

CloudWatch is an essential tool for monitoring and logging in AWS, offering a robust set of features to ensure the health and performance of your applications and infrastructure.



```
 wget https://amazoncloudwatch-agent.s3.amazonaws.com/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm
 sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```
```
 sudo rpm -U ./amazon-cloudwatch-agent.rpm
 sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
 sudo amazon-cloudwatch-agent-ctl -a start -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json -s

 ```

 # Custom Metrics

## Overview:
Custom metrics in CloudWatch allow you to define and monitor metrics that are not automatically provided by AWS services. For instance, you can send application-specific data, memory usage, or disk space to CloudWatch for monitoring.

## Example: Adding Custom Metrics Using CloudWatch Agent

### Step-by-Step:

1. **Install CloudWatch Agent**
   - Install the CloudWatch Agent on your EC2 instance:
     ```bash
     sudo yum install amazon-cloudwatch-agent -y  # For Amazon Linux/Red Hat
     sudo apt-get install amazon-cloudwatch-agent -y  # For Ubuntu/Debian
     ```

2. **Create a Configuration File**
   - Create a JSON configuration file for the CloudWatch Agent:
     ```bash
     sudo nano /opt/aws/amazon-cloudwatch-agent/bin/config.json
     ```
   - Example configuration to monitor memory and disk space:
     ```json
     {
       "metrics": {
         "append_dimensions": {
           "InstanceId": "${aws:InstanceId}"
         },
         "metrics_collected": {
           "mem": {
             "measurement": ["mem_used_percent"]
           },
           "disk": {
             "resources": ["/"],
             "measurement": ["used_percent"]
           }
         }
       }
     }
     ```

3. **Start the CloudWatch Agent**
   - Apply the configuration and start the agent:
     ```bash
     sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
         -a fetch-config \
         -m ec2 \
         -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
         -s
     ```

4. **Verify Metrics in CloudWatch**
   - Navigate to the **CloudWatch Console** > **Metrics**.
   - Check for the custom namespace (e.g., `CWAgent`) and verify your metrics.

---

# CloudWatch Logging

## Overview:
CloudWatch Logs allow you to monitor, store, and access log files from EC2 instances, applications, or other AWS services.

### Practical: Storing HTTPD Server Logs in Log Groups

1. **Install and Configure Apache HTTPD Server**
   - Install Apache:
     ```bash
     sudo yum install httpd -y  # For Amazon Linux/Red Hat
     sudo apt-get install apache2 -y  # For Ubuntu/Debian
     ```
   - Start the service:
     ```bash
     sudo systemctl start httpd
     sudo systemctl enable httpd
     ```

2. **Configure CloudWatch Agent for HTTPD Logs**
   - Update the CloudWatch Agent configuration to include Apache logs:
     ```bash
     sudo nano /opt/aws/amazon-cloudwatch-agent/bin/config.json
     ```
   - Example configuration:
     ```json
     {
       "logs": {
         "logs_collected": {
           "files": {
             "collect_list": [
               {
                 "file_path": "/var/log/httpd/access_log",
                 "log_group_name": "httpd-access-logs",
                 "log_stream_name": "{instance_id}"
               },
               {
                 "file_path": "/var/log/httpd/error_log",
                 "log_group_name": "httpd-error-logs",
                 "log_stream_name": "{instance_id}"
               }
             ]
           }
         },
         "log_stream_name": "default-log-stream"
       }
     }
     ```

3. **Restart CloudWatch Agent**
   - Apply the updated configuration and restart the agent:
     ```bash
     sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
         -a fetch-config \
         -m ec2 \
         -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
         -s
     ```

4. **View Logs in CloudWatch**
   - Go to **CloudWatch Console** > **Log Groups**.
   - Select the log group (e.g., `httpd-access-logs`) to view and analyze the logs.

---

# SNS Topic

## Overview:
Amazon Simple Notification Service (SNS) is a fully managed messaging service for communication between distributed systems, microservices, and end-users.

### Practical Example:

1. **Create an SNS Topic**
   - Navigate to the **SNS Console**.
   - Click **Create topic**.
   - Provide a name and select the type (Standard or FIFO).

2. **Subscribe to the Topic**
   - Add subscriptions (e.g., email, SMS, or Lambda function).
   - Verify the subscription through the confirmation link sent to the email/SMS.

3. **Publish a Message**
   - From the **SNS Console**, select the topic and click **Publish message**.
   - Add a subject and message body, then click **Publish**.

4. **Automate Notifications**
   - Integrate the SNS topic with CloudWatch Alarms, Lambda, or other AWS services to trigger alerts automatically.

---


#  Relational Database Services (RDS) 

## In-Depth Guide to RDS, DBMS, SQL, and MariaDB

## 1. Introduction to Amazon RDS
Amazon Relational Database Service (RDS) is a managed database solution provided by AWS that simplifies the setup, operation, and scaling of relational databases in the cloud. It supports various database engines, including MySQL, PostgreSQL, MariaDB, Oracle, and Microsoft SQL Server.

### 1.1 Key Features of RDS:
- **Automated Management:** AWS handles backups, patching, and monitoring.
- **Scalability:** Storage and compute capacity can be adjusted dynamically.
- **High Availability:** Multi-AZ deployments ensure minimal downtime.
- **Security:** Integration with IAM, encryption, and VPC for secure access.
- **Read Replicas:** Improves performance for read-heavy applications.
- **Performance Insights:** Analyzes database activity for optimization.

### 1.2 RDS Deployment Options
- **Single-AZ Deployment:** Cost-effective but has downtime during maintenance.
- **Multi-AZ Deployment:** Ensures high availability with automatic failover.
- **Read Replicas:** Used for scaling read-heavy workloads.
- **Aurora RDS:** AWS’s cloud-native relational database with higher performance.

### 1.3 RDS Pricing Models
- **On-Demand Pricing:** Pay per hour with no long-term commitments.
- **Reserved Instances:** Lower cost for long-term commitments.
- **Serverless RDS:** Scales automatically based on demand.

---
## 2. Advantages of Amazon RDS

### 2.1 Ease of Management
- **Automated Backups:** Daily snapshots for easy recovery.
- **Monitoring & Logging:** AWS CloudWatch, CloudTrail, and Performance Insights.
- **Maintenance Window:** Allows scheduled updates with minimal impact.

### 2.2 High Availability and Disaster Recovery
- **Multi-AZ Deployments:** Standby instance automatically takes over in case of failure.
- **Automated Failover:** Ensures business continuity.
- **Point-in-Time Recovery:** Restores database to any time within retention period.

### 2.3 Security and Compliance
- **IAM Integration:** Fine-grained access control.
- **Encryption:** Data encrypted at rest and in transit.
- **VPC Isolation:** Controls access via private networking.
- **Compliance Standards:** Meets regulatory requirements (HIPAA, GDPR, PCI-DSS, etc.).

### 2.4 Performance Optimization
- **Provisioned IOPS:** Ensures high performance with SSD-backed storage.
- **Read Replicas:** Distributes read operations across multiple instances.
- **Automatic Storage Scaling:** Adjusts based on workload.

---
## 3. Introduction to DBMS (Database Management System)
A Database Management System (DBMS) is software used to store, retrieve, and manage data in a structured manner. It serves as an intermediary between the database and users or applications.

### 3.1 Types of DBMS
- **Relational DBMS (RDBMS):** Uses tables with rows and columns (e.g., MySQL, PostgreSQL, MariaDB).
- **NoSQL DBMS:** Handles unstructured data (e.g., MongoDB, Cassandra).
- **Hierarchical DBMS:** Organizes data in a tree-like structure (e.g., IBM IMS).
- **Network DBMS:** Uses graph-based structures for relationships (e.g., IDS, IDMS).

### 3.2 Key Functions of DBMS
- **Data Storage & Retrieval:** Efficiently stores and fetches data.
- **Concurrency Control:** Manages multiple users accessing data simultaneously.
- **Data Integrity:** Ensures consistency and accuracy of data.
- **Backup & Recovery:** Provides data redundancy and recovery mechanisms.
- **Security Management:** Implements authentication and authorization.

### 3.3 Comparison of DBMS vs. File System
| Feature | DBMS | File System |
|---------|------|-------------|
| Structure | Organized tables | Flat files |
| Querying | Uses SQL | Manual search |
| Security | High | Low |
| Backup & Recovery | Automated | Manual |

---
## 4. Structured Query Language (SQL)
SQL (Structured Query Language) is the standard language for managing relational databases. It consists of multiple components:

### 4.1 SQL Categories
- **Data Definition Language (DDL):** `CREATE`, `ALTER`, `DROP`
- **Data Manipulation Language (DML):** `INSERT`, `UPDATE`, `DELETE`
- **Data Query Language (DQL):** `SELECT`
- **Data Control Language (DCL):** `GRANT`, `REVOKE`
- **Transaction Control Language (TCL):** `COMMIT`, `ROLLBACK`

### 4.2 Common SQL Commands
- **Create a Table:**
```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    grade CHAR(1)
);
```

- **Insert Data:**
```sql
INSERT INTO students (id, name, age, grade) VALUES (1, 'John Doe', 20, 'A');
```

- **Retrieve Data:**
```sql
SELECT * FROM students WHERE grade = 'A';
```

- **Update Data:**
```sql
UPDATE students SET age = 21 WHERE id = 1;
```

- **Delete Data:**
```sql
DELETE FROM students WHERE id = 1;
```

---
## 5. Introduction to MariaDB
MariaDB is an open-source relational database management system (RDBMS) that is a fork of MySQL. It offers enhanced performance, security, and compatibility with MySQL.

### 5.1 Key Features of MariaDB
- **Open-Source & Community-Driven:** No licensing fees.
- **Performance Improvements:** Faster execution of complex queries.
- **Storage Engines:** Supports InnoDB, MyISAM, Aria, and more.
- **Security Enhancements:** Stronger authentication mechanisms.

### 5.2 Installing MariaDB
```bash
sudo apt update
sudo apt install mariadb-server -y
```

### 5.3 Configuring MariaDB
- **Start MariaDB Service:**
```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
- **Secure Installation:**
```bash
sudo mysql_secure_installation
```

### 5.4 Connecting to MariaDB
```bash
mysql -u root -p
```

### 5.5 Creating a Database in MariaDB
```sql
CREATE DATABASE student_db;
```

---
## 🎯 Conclusion
This guide provides an in-depth understanding of Amazon RDS, its advantages, and its role in cloud-based database management. We also explored fundamental database concepts, SQL operations, and MariaDB’s capabilities. Mastering these topics will help in efficient database design, performance optimization, and security best practices.



# 🌐  Content Delivery with CloudFront 🌐 

## Introduction to CloudFront
AWS CloudFront is a global content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to users with low latency and high transfer speeds. It works by caching content at edge locations worldwide.

### Benefits of CloudFront:
- **Reduced Latency**: Serves content from the nearest edge location to the user.
- **Improved Security**: Integrates with AWS Shield, AWS WAF, and HTTPS encryption.
- **Cost Optimization**: Reduces data transfer costs with caching mechanisms.
- **Scalability**: Handles traffic spikes efficiently without additional infrastructure.

---

##   CloudFront + S3 Integration to Host Frontend of Website

### Step 1: Create an S3 Bucket
1. Navigate to the **AWS S3 Console**.
2. Click **Create bucket**.
3. Enter a **unique bucket name**.
4. Choose **Region**.
5. **Disable Block Public Access** (for public website hosting).
6. Click **Create bucket**.

### Step 2: Upload Website Files
1. Open the created bucket.
2. Click **Upload**.
3. Select your website frontend files (HTML, CSS, JS, images).
4. Click **Upload**.

### Step 3: Enable Static Website Hosting
1. Open your **S3 Bucket**.
2. Go to **Properties**.
3. Scroll down to **Static website hosting**.
4. Select **Enable**.
5. Set the **index document** (e.g., `index.html`).
6. Click **Save changes**.

### Step 4: Create a CloudFront Distribution
1. Navigate to the **AWS CloudFront Console**.
2. Click **Create Distribution**.
3. Select **Web** as delivery method.
4. Under **Origin Settings**:
   - **Origin Domain Name**: Choose the S3 bucket.
   - **Origin Access Control (OAC)**: Enable OAC for security.
   - **Restrict Bucket Access**: Yes.
5. Set **Default Cache Behavior**:
   - Viewer Protocol Policy: Redirect HTTP to HTTPS.
   - Allowed HTTP Methods: GET, HEAD, OPTIONS.
6. Click **Create Distribution**.

### Step 5: Update S3 Bucket Policy
1. Open the S3 bucket.
2. Go to **Permissions** → **Bucket Policy**.
3. Add the following policy to allow CloudFront to access S3:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudfront.amazonaws.com"
         },
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-bucket-name/*"
       }
     ]
   }
   ```
4. Replace `your-bucket-name` with your actual bucket name.
5. Click **Save**.

### Step 6: Access Your Website via CloudFront
- Copy the **CloudFront Domain Name** from the AWS CloudFront console.
- Open a browser and enter the CloudFront URL.
- Your website is now delivered via CloudFront globally! 🚀

---

## Best Practices
- **Enable Logging**: Use CloudFront access logs for monitoring.
- **Use SSL/TLS**: Secure your website using HTTPS with an SSL certificate.
- **Cache Optimization**: Configure cache policies for efficient content delivery.
- **Restrict Direct S3 Access**: Ensure users access content only via CloudFront.









# 🌐 AWS Route 53 — Routing Policies & Record Types

## 📌 What is Amazon Route 53?

**Amazon Route 53** is AWS's **DNS (Domain Name System) service**.

DNS converts a domain name into an IP address or helps route traffic to an AWS resource.

### 🔹 Example

    User
      ↓
    www.example.com
      ↓
    Route 53
      ↓
    Application Load Balancer
      ↓
    Application

👉 **Simple Definition:**

> Route 53 is AWS's DNS service used to route users to the correct application, server, or AWS resource.

---

# 🛣️ Route 53 Routing Policies

A **Routing Policy** tells Route 53:

> **"How should I decide where to send the user's traffic?"**

AWS Route 53 provides the following routing policies:

1. 🟢 Simple Routing
2. ⚖️ Weighted Routing
3. 🚀 Latency-Based Routing
4. 🔄 Failover Routing
5. 🌍 Geolocation Routing
6. 📍 Geoproximity Routing
7. 🌐 IP-Based Routing
8. 🔀 Multivalue Answer Routing

---

# 1️⃣ Simple Routing

## 📌 Meaning

Simple Routing is used when you have **one resource** and want Route 53 to send traffic to that resource.

### 🔹 Architecture

    User
      ↓
    Route 53
      ↓
    ALB
      ↓
    Application

### 🔹 Example Record

    Name: www.example.com
    Type: A
    Routing Policy: Simple
    Target: Application Load Balancer

### 🔹 Use Cases

Simple Routing is useful when:

- You have a single application
- You don't need traffic distribution
- You don't need failover
- You have a basic DNS requirement

### 🧠 Remember

> **Simple = One resource**

---

# 2️⃣ Weighted Routing ⚖️

## 📌 Meaning

Weighted Routing distributes traffic between multiple resources based on **weights**.

### 🔹 Example

Suppose you have two versions of an application:

    ALB-1 → Old Application
    ALB-2 → New Application

You want:

    90% → Old Application
    10% → New Application

### 🔹 Architecture

                    Route 53
                       ↓
                Weighted Routing
                   /       \
                 90%       10%
                  ↓          ↓
                ALB-1      ALB-2
                 ↓           ↓
                Old         New
              Version      Version

### 🔹 Example

You are releasing a new application version.

Instead of sending 100% traffic to the new version, you can send:

    Old Version → 90%
    New Version → 10%

If the new version works correctly, you can gradually change:

    90% / 10%
       ↓
    80% / 20%
       ↓
    60% / 40%
       ↓
    50% / 50%
       ↓
    0% / 100%

### 🎯 Use Case

Weighted Routing is commonly useful for:

- Canary deployments
- Blue/Green-style traffic shifting
- Testing new application versions
- Gradually moving traffic

### 🧠 Remember

> **Weighted = Traffic distribution based on weight**

---

# 3️⃣ Latency-Based Routing 🚀

## 📌 Meaning

Latency-Based Routing sends users to the resource that provides the **lowest network latency** among the configured resources.

Suppose your application is deployed in multiple AWS regions:

    🇮🇳 Mumbai
    🇸🇬 Singapore
    🇬🇧 London

### 🔹 Architecture

                       Route 53
                          ↓
              ┌───────────┼───────────┐
              ↓           ↓           ↓
           Mumbai      Singapore    London
              ↓           ↓           ↓
           Server        Server       Server

### 🔹 Example

An Indian user:

    India User
        ↓
    Route 53
        ↓
    Mumbai Region
        ↓
    Application

Another user may be routed to another region if that region provides lower latency for that user.

### 🎯 Use Case

Use Latency-Based Routing when:

- Application is deployed in multiple AWS regions
- Users are geographically distributed
- You want to reduce network latency
- You want users to access a region with lower latency

### 🧠 Remember

> **Latency = Route users based on lowest network latency**

---

# 4️⃣ Failover Routing 🔄

## 📌 Meaning

Failover Routing is used for **High Availability** and **Disaster Recovery**.

You normally have:

    Primary   → Mumbai
    Secondary → Singapore

### 🔹 Normal Situation

    User
      ↓
    Route 53
      ↓
    Primary
      ↓
    Mumbai
      ↓
    Application

### 🔹 If Primary Fails

    User
      ↓
    Route 53
      ↓
    Secondary
      ↓
    Singapore
      ↓
    Application

### 🔹 Architecture

                    Route 53
                       ↓
                 Health Check
                       ↓
                ┌──────┴──────┐
                ↓             ↓
             Healthy       Unhealthy
                ↓             ↓
             Mumbai        Singapore
             Primary       Secondary

### 🏥 Health Check

Route 53 can use health checks to determine whether the primary resource is healthy.

If:

    Primary = Healthy
        ↓
    Traffic → Primary

If:

    Primary = Unhealthy
        ↓
    Traffic → Secondary

### 🎯 Use Cases

Failover Routing is useful for:

- Disaster Recovery
- High Availability
- Backup application
- Multi-region applications

### 🧠 Remember

> **Failover = Primary fails → Secondary receives traffic**

---

# 5️⃣ Geolocation Routing 🌍

## 📌 Meaning

Geolocation Routing routes users based on their **geographic location**.

### 🔹 Example

    🇮🇳 India Users → Mumbai Server
    🇺🇸 USA Users   → USA Server
    🇬🇧 UK Users    → London Server

### 🔹 Architecture

                       Route 53
                          ↓
              ┌───────────┼───────────┐
              ↓           ↓           ↓
           🇮🇳 India    🇺🇸 USA     🇬🇧 UK
              ↓           ↓           ↓
           Mumbai        USA        London

### 🔹 Example

If a user is located in India:

    India User
        ↓
    Route 53
        ↓
    India/Mumbai Resource

If a user is located in the USA:

    USA User
        ↓
    Route 53
        ↓
    USA Resource

### 🎯 Use Cases

Geolocation Routing can be useful when:

- You want different content for different locations
- You need geographic traffic routing
- You want users from different countries to access different endpoints

### 🧠 Remember

> **Geolocation = Based on user's geographic location**

---

# 6️⃣ Geoproximity Routing 📍

## 📌 Meaning

Geoproximity Routing routes traffic based on the **geographic location of users and resources**.

It can also use a **bias** to expand or reduce the geographic area from which traffic is routed to a particular resource.

### 🔹 Example

Suppose you have:

    Mumbai Server
    Singapore Server

Route 53 considers the geographic relationship between the user and the resources.

### 🔹 Architecture

    User Location
          +
    Resource Location
          +
        Bias
          ↓
      Route 53
          ↓
    Selected Resource

### 🔹 Example

    User
      ↓
    Route 53
      ↓
    Geographic Proximity
      ↓
    Appropriate Resource

### 🔥 Geolocation vs Geoproximity

### Geolocation

Based on the **user's geographic location**.

    India → Mumbai
    USA   → USA
    UK    → London

### Geoproximity

Based on **geographic proximity between users and resources**, with optional bias.

    User Location
         +
    Resource Location
         +
       Bias
         ↓
    Routing Decision

### 🧠 Remember

> **Geoproximity = Geographic proximity + optional bias**

---

# 7️⃣ IP-Based Routing 🌐

## 📌 Meaning

IP-Based Routing routes traffic based on the **source IP address**.

You can configure IP ranges and associate them with specific endpoints.

### 🔹 Example

    IP Range: 10.0.0.0/8
         ↓
      Server A

    IP Range: 192.168.0.0/16
         ↓
      Server B

### 🔹 Architecture

    User
      ↓
    Source IP
      ↓
    Route 53
      ↓
    Check IP Range
      ↓
    Specific Endpoint

### 🎯 Use Cases

IP-Based Routing can be useful when:

- Specific IP ranges should go to specific resources
- You need IP-based traffic mapping
- You have controlled network ranges

### 🧠 Remember

> **IP-Based = Route traffic based on source IP**

---

# 8️⃣ Multivalue Answer Routing 🔀

## 📌 Meaning

Multivalue Answer Routing returns **multiple healthy records** in response to a DNS query.

Suppose you have:

    Server A → 10.0.0.10
    Server B → 10.0.0.20
    Server C → 10.0.0.30

Route 53 can return multiple IP addresses.

### 🔹 Example

    example.com
         ↓
    10.0.0.10
    10.0.0.20
    10.0.0.30

Route 53 can also associate health checks with records.

If one endpoint becomes unhealthy, it can be excluded from the DNS response.

### ⚠️ Important

Multivalue Answer Routing is **not a replacement for a Load Balancer**.

### 🧠 Remember

> **Multivalue = Return multiple healthy answers**

---

# 📊 Routing Policies — Quick Comparison

| Routing Policy | Main Purpose | Example |
|---|---|---|
| 🟢 Simple | Single resource | User → ALB |
| ⚖️ Weighted | Traffic distribution | 90% V1, 10% V2 |
| 🚀 Latency | Lowest latency | User → Low-latency region |
| 🔄 Failover | Disaster recovery | Primary → Secondary |
| 🌍 Geolocation | User location | India → Mumbai |
| 📍 Geoproximity | Geographic proximity | User → Appropriate nearby resource |
| 🌐 IP-Based | Source IP | IP Range → Endpoint |
| 🔀 Multivalue | Multiple healthy answers | Return multiple IPs |

---

# 📝 Route 53 Record Types

A **DNS Record** tells Route 53 what a domain should resolve to or what information it should provide.

Common Route 53 record types:

1. 🔵 A Record
2. 🟣 AAAA Record
3. 🔗 CNAME Record
4. 📧 MX Record
5. 📝 TXT Record
6. 🌐 NS Record
7. ⚙️ SOA Record
8. ⭐ Alias Record

---

# 1️⃣ A Record

## 📌 Meaning

**A Record = Domain Name → IPv4 Address**

### 🔹 Example

    example.com
         ↓
    13.234.100.20

### 🔹 Record

    Name: example.com
    Type: A
    Value: 13.234.100.20

### 🧠 Remember

> **A = IPv4 Address**

---

# 2️⃣ AAAA Record

## 📌 Meaning

**AAAA Record = Domain Name → IPv6 Address**

### 🔹 Example

    example.com
         ↓
    2001:db8::1

### 🧠 Difference

    A     → IPv4
    AAAA  → IPv6

### 🧠 Remember

> **AAAA = IPv6 Address**

---

# 3️⃣ CNAME Record 🔗

## 📌 Meaning

**CNAME = Canonical Name**

It maps one domain name to another domain name.

### 🔹 Example

    www.example.com
          ↓
    app.example.com

### 🔹 Record

    Name: www.example.com
    Type: CNAME
    Value: app.example.com

Another example:

    api.example.com
          ↓
    my-api.example.net

### ⚠️ Important

CNAME points to a **domain name**, not directly to an IP address.

### 🧠 Remember

> **CNAME = Name → Name**

---

# 4️⃣ MX Record 📧

## 📌 Meaning

**MX = Mail Exchange**

MX records specify the mail servers responsible for receiving email for a domain.

### 🔹 Example

    example.com
         ↓
    MX Record
         ↓
    Mail Server

For example:

    user@example.com
          ↓
       DNS
          ↓
    MX Record
          ↓
     Mail Server

### 🎯 Use Case

MX records are used for:

- Email delivery
- Mail server configuration
- Email services

### 🧠 Remember

> **MX = Mail Server**

---

# 5️⃣ TXT Record 📝

## 📌 Meaning

TXT records store **text information** in DNS.

### 🔹 Common Uses

- Domain verification
- SPF
- DKIM-related configuration
- Security-related DNS information

### 🔹 Example

    example.com
         ↓
       TXT
         ↓
    "google-site-verification=xxxxx"

### 🧠 Remember

> **TXT = Text Information**

---

# 6️⃣ NS Record 🌐

## 📌 Meaning

**NS = Name Server**

NS records specify the authoritative name servers for a domain.

### 🔹 Example

    example.com
         ↓
    Name Servers
         ↓
    ns-123.awsdns-xx.com
    ns-456.awsdns-xx.net

When you create a Route 53 Hosted Zone, AWS provides name servers.

### 🧠 Remember

> **NS = Name Server**

---

# 7️⃣ SOA Record ⚙️

## 📌 Meaning

**SOA = Start of Authority**

SOA contains important information about a DNS zone.

It includes information such as:

- Primary name server
- Administrative information
- Serial number
- Refresh interval
- Retry interval
- Expire time

### 🧠 Remember

> **SOA = Information about the DNS Zone**

---

# 8️⃣ Alias Record ⭐

## 📌 Meaning

An **Alias** is an AWS Route 53 feature that allows you to point a domain to supported AWS resources.

### 🔹 Example

    www.example.com
          ↓
       Route 53
          ↓
    Application Load Balancer
          ↓
       Application

### 🔹 Example Record

    Name: www.example.com
    Type: A
    Alias: Yes
    Target: Application Load Balancer

### 🔹 Alias can point to supported AWS resources such as:

- Application Load Balancer
- Network Load Balancer
- CloudFront
- API Gateway
- S3 website endpoint
- Other supported AWS resources

---

# ⚔️ CNAME vs Alias

This is a **very common AWS interview question**.

## 🔗 CNAME

CNAME points one domain name to another domain name.

    www.example.com
          ↓
    app.example.com

Example:

    Name: www.example.com
    Type: CNAME
    Value: app.example.com

---

## ⭐ Alias

Alias can point a Route 53 record to supported AWS resources.

    www.example.com
          ↓
    Application Load Balancer

Example:

    Name: www.example.com
    Type: A
    Alias: Yes
    Target: Application Load Balancer

---

# 🔥 Important Difference: CNAME vs Alias

| Feature | CNAME | Alias |
|---|---|---|
| Points to another domain | ✅ Yes | ❌ Not its main purpose |
| Points to AWS resources | ❌ No | ✅ Yes |
| Points directly to IP | ❌ No | AWS resource target |
| Route 53 specific | ❌ No | ✅ Yes |
| Can be used for root domain | ❌ Generally no | ✅ Yes, for supported AWS targets |
| Example | www → app.example.com | example.com → ALB |

### 🧠 Remember

> **CNAME = Domain → Domain**

> **Alias = Domain → Supported AWS Resource**

---

# 🚀 Real-World DevOps Example

Suppose your application is running on **Amazon EKS**.

Architecture:

    👤 User
       ↓
    www.myapp.com
       ↓
    🌐 Route 53
       ↓
    ⚖️ Application Load Balancer
       ↓
    Kubernetes Ingress
       ↓
    Kubernetes Service
       ↓
    🚀 Pods
       ↓
    Application

You can create a Route 53 record:

    Name: www.myapp.com
    Type: A
    Alias: Yes
    Target: Application Load Balancer
    Routing Policy: Simple

---

# 🚀 Real-World Weighted Routing Example

Suppose you have:

    ALB-1 → Old Application
    ALB-2 → New Application

You can configure:

    www.myapp.com
          ↓
       Route 53
          ↓
    Weighted Routing
       /       \
     90%       10%
      ↓          ↓
    ALB-1      ALB-2
     Old        New

This allows you to test the new version with a small percentage of traffic.

---

# 🔄 Real-World Failover Example

Suppose you have:

    Primary Region   → Mumbai
    Secondary Region → Singapore

Architecture:

                    Route 53
                       ↓
                 Health Check
                       ↓
                ┌──────┴──────┐
                ↓             ↓
             Healthy       Unhealthy
                ↓             ↓
             Mumbai        Singapore
             Primary       Secondary

If Mumbai fails:

    Mumbai ❌
       ↓
    Route 53
       ↓
    Singapore ✅
       ↓
    Application

---

# 🧠 Routing Policy vs Record Type

This is important to understand.

## 🛣️ Routing Policy

Routing Policy decides:

> **HOW should Route 53 route the traffic?**

Examples:

    Simple
    Weighted
    Latency
    Failover
    Geolocation
    Geoproximity
    IP-Based
    Multivalue

---

## 📝 Record Type

Record Type defines:

> **WHAT type of DNS information are we storing?**

Examples:

    A
    AAAA
    CNAME
    MX
    TXT
    NS
    SOA
    Alias

---

# 🔥 Both Work Together

For example:

    Name: www.myapp.com
    Type: A
    Alias: Yes
    Target: ALB
    Routing Policy: Weighted

Here:

    Type A
       ↓
    DNS record type

    Alias
       ↓
    Points to AWS ALB

    Weighted
       ↓
    Controls how traffic is distributed

---

# 🎯 Easy Interview Answer

### ❓ What is Route 53?

> Amazon Route 53 is AWS's highly available and scalable DNS service used for domain name resolution and traffic routing.

### ❓ What is a Routing Policy?

> Routing Policy defines how Route 53 decides where to route DNS queries or user traffic.

### ❓ What is a Record Type?

> A DNS record defines what information a domain name should resolve to or provide.

### ❓ What is an A Record?

> An A record maps a domain name to an IPv4 address.

### ❓ What is an AAAA Record?

> An AAAA record maps a domain name to an IPv6 address.

### ❓ What is CNAME?

> CNAME maps one domain name to another domain name.

### ❓ What is Alias?

> Alias is a Route 53 feature that allows a domain to point to supported AWS resources such as an ALB, CloudFront distribution, or S3 website endpoint.

### ❓ What is Weighted Routing?

> Weighted Routing distributes traffic between multiple resources based on configured weights.

### ❓ What is Latency Routing?

> Latency-Based Routing sends users to the resource that provides the lowest network latency.

### ❓ What is Failover Routing?

> Failover Routing sends traffic to a secondary resource when the primary resource becomes unhealthy.

---

# 🧠 Final Memory Trick

## 🛣️ Routing Policies

    🟢 Simple
       → One resource

    ⚖️ Weighted
       → Percentage / Traffic Distribution

    🚀 Latency
       → Lowest Latency

    🔄 Failover
       → Primary / Backup

    🌍 Geolocation
       → User Location

    📍 Geoproximity
       → Geographic Proximity + Bias

    🌐 IP-Based
       → Source IP

    🔀 Multivalue
       → Multiple Healthy Answers


## 📝 Record Types

    🔵 A
       → IPv4

    🟣 AAAA
       → IPv6

    🔗 CNAME
       → Domain → Domain

    📧 MX
       → Mail Server

    📝 TXT
       → Text / Verification

    🌐 NS
       → Name Server

    ⚙️ SOA
       → DNS Zone Information

    ⭐ Alias
       → Supported AWS Resource


# 🚀 One-Line Summary

> **Route 53 = DNS + Traffic Routing**

> **Record Type = WHAT**

> **Routing Policy = HOW**

### Example:

    www.myapp.com
          ↓
       Route 53
          ↓
    A + Alias Record
          ↓
          ALB
          ↓
    Weighted Routing
       /       \
     90%       10%
      ↓          ↓
     Old        New
     App        App
