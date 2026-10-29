# Linux Notes for DevOps Interviews and Practical Work

These notes cover Linux fundamentals, essential commands, file permissions, users, processes, networking, logs, SSH, and troubleshooting. The examples are especially useful for your Jenkins, Docker, Terraform, and AWS EC2 practice.

## 1. What is Linux?

Linux is an open-source operating system kernel. Linux distributions such as Ubuntu, Debian, and Amazon Linux combine the kernel with system utilities and other software to create a complete operating system.

Why is Linux important in DevOps?

- Hosts Jenkins, Docker, Kubernetes tools, and web applications.
- Runs cloud servers such as AWS EC2 instances.
- Supports shell scripting and deployment automation.
- Provides tools for monitoring processes, logs, networking, and system resources.

Interview answer: “Linux is widely used in DevOps because it provides a stable environment for hosting applications, automating tasks through shell scripts, managing infrastructure, and troubleshooting servers.”

## 2. Linux file system structure

`/` — Root directory

`/home`

Regular users' home directories

`/root`

Root user's home directory

`/etc`

System configuration files

`/var/log`

Many system and application logs

`/var/lib`

Persistent application and service data

`/tmp`

Temporary files

`/usr/bin`

Many user commands and executables

`/opt`

Optional or third-party software

Other important directories:

- `/bin` and `/sbin`: Essential commands; on many modern distributions, these are links into `/usr`.
- `/dev`: Device files.
- `/proc`: Virtual filesystem exposing process and kernel information.
- `/sys`: Kernel and device information.
- `/boot`: Files required for booting.
- `/mnt` and `/media`: Common mount points for filesystems and removable media.
- `/var/lib/jenkins`: Common Jenkins home directory on Linux package installations, though it can be configured differently.

## 3. Basic Linux commands

### Navigation and files

| Command  | Purpose                                   | Example              |
| -------- | ----------------------------------------- | -------------------- |
| `pwd`    | Show current directory                    | `pwd`                |
| `ls`     | List files                                | `ls -l`              |
| `ls -la` | Include hidden files                      | `ls -la`             |
| `cd`     | Change directory                          | `cd /var/log`        |
| `cd ..`  | Go to parent directory                    | `cd ..`              |
| `mkdir`  | Create directory                          | `mkdir project`      |
| `touch`  | Create an empty file or update timestamps | `touch app.log`      |
| `cp`     | Copy files                                | `cp a.txt b.txt`     |
| `mv`     | Move or rename files                      | `mv old.txt new.txt` |
| `rm`     | Delete files                              | `rm file.txt`        |
| `rmdir`  | Remove an empty directory                 | `rmdir olddir`       |
| `file`   | Identify file type                        | `file app.jar`       |

Be careful with `rm -r` and especially `rm -rf`; they can permanently delete large directory trees.

### Reading and searching files

```
cat file.txt
less file.txt
head -n 20 file.txt
tail -n 50 app.log
tail -f app.log
grep "ERROR" app.log
grep -in "failed" app.log
find /var/log -name "*.log"
```

- `cat`: Display file content.
- `less`: Read a file page by page.
- `head` / `tail`: Display the beginning or end of a file.
- `tail -f`: Follow new log entries.
- `grep`: Search for matching text.
- `find`: Search for files and directories.

DevOps example: If your Spring Boot application fails to start, inspect its container logs or application log files and search for `ERROR`, `Exception`, or `Caused by`.

## 4. Linux file permissions

Linux permissions determine who can read, modify, or execute files.

There are three permission categories:

- Owner (`u`) — the file's owner.
- Group (`g`) — users in the assigned group.
- Others (`o`) — all other users.

| Permission    | Letter | Numeric value |
| ------------- | ------ | ------------- |
| Read          | `r`    | 4             |
| Write         | `w`    | 2             |
| Execute       | `x`    | 1             |
| No permission | `-`    | 0             |

For example, `-rwxr-xr--` means the owner can read, write, and execute; the group can read and execute; others can only read.

### Common permission commands

```
ls -l
chmod 755 script.sh
chmod 644 config.txt
chmod +x deploy.sh
chown ubuntu:ubuntu deploy.sh
```

- `755`: Owner has `rwx`; group and others have `r-x`.
- `644`: Owner has `rw-`; group and others have `r--`.
- `chmod +x`: Adds execute permission according to the command's default target.
- `chown`: Changes file ownership.

Important for Jenkins: If Jenkins cannot access a file or Docker socket, investigate the owner, group membership, directory permissions, and service configuration. Avoid using `chmod 777` as a shortcut.

## 5. Users and groups

Linux supports multiple users, each with its own permissions and access.

```
whoami
id
who
groups
sudo adduser devuser
sudo usermod -aG docker devuser
sudo passwd devuser
```

These commands show the current user, user and group IDs, logged-in users, group membership, create a user, add a user to a group, and set a password.

On some distributions, user-management commands differ. For example, `useradd` is available on many distributions, while `adduser` may be an interactive wrapper.

What is `sudo`?

`sudo` runs a command with elevated privileges when the user's policy permits it.

```
sudo systemctl status jenkins
sudo journalctl -u jenkins -n 100
```

Be cautious when granting administrator access or adding users to the `docker` group: Docker access can effectively grant root-level control of the host.

## 6. Processes and services

A process is a running program. A service is software managed to run in the background, often through `systemd`.

### Process commands

```
ps aux
top
pgrep -af java
kill PID
```

- `ps aux`: List running processes.
- `top`: Monitor processes and resource usage interactively.
- `pgrep -af java`: Search for processes whose command lines match `java`.
- `kill PID`: Send the default termination signal to a process.

Use `kill -9 PID` only as a last resort because it does not allow the process to shut down gracefully.

### Service management with systemd

```
sudo systemctl status jenkins
sudo systemctl start jenkins
sudo systemctl stop jenkins
sudo systemctl restart jenkins
sudo systemctl enable jenkins
sudo systemctl disable jenkins
```

- `start`: Start a service now.
- `stop`: Stop a service.
- `restart`: Restart a service.
- `enable`: Configure the service to start automatically at boot.
- `status`: Show service state and recent status messages.

### View service logs

```
sudo journalctl -u jenkins
sudo journalctl -u jenkins -n 100
sudo journalctl -u jenkins -f
```

These are useful when Jenkins is down, a service fails to start, or a deployment command behaves unexpectedly.

## 7. Disk, memory, and CPU monitoring

| Command        | Purpose                                         |
| -------------- | ----------------------------------------------- |
| `df -h`        | Check filesystem space                          |
| `du -sh /path` | Check directory size                            |
| `free -h`      | Check memory usage                              |
| `uptime`       | Show uptime and load averages                   |
| `top`          | Inspect processes and system load               |
| `lscpu`        | Show CPU details                                |
| `lsblk`        | Show block devices and partitions               |
| `vmstat 1`     | Display system activity at one-second intervals |

Example:

```
df -h
free -h
du -sh /var/lib/jenkins
```

If a Jenkins build fails with “No space left on device,” check `df -h` first. Use `du -sh` to identify large directories. Do not delete Jenkins workspaces, Docker data, or logs blindly because they may contain important data.

## 8. Networking commands

These commands are particularly important when an EC2-hosted application is unreachable.

```
ip addr
ip route
ss -tulpn
curl -I http://localhost:8080
curl -v http://SERVER_IP:8080
getent hosts example.com
ping -c 4 8.8.8.8
```

- `ip addr`: Displays network interfaces and addresses.
- `ip route`: Displays routing information.
- `ss -tulpn`: Shows listening TCP/UDP sockets and, where permitted, process information.
- `curl`: Tests HTTP connectivity.
- `getent hosts`: Checks name resolution through the configured system resolver.
- `ping`: Tests ICMP reachability when allowed.

For your AWS projects, an application working on `localhost:8080` does not automatically mean it is accessible externally. Check the application bind address, host firewall, EC2 security group, subnet routes, and public addressing.

## 9. Package management

Package commands depend on the Linux distribution.

| Task                     | Ubuntu / Debian          | Amazon Linux / RHEL family |
| ------------------------ | ------------------------ | -------------------------- |
| Refresh package metadata | `sudo apt update`        | `sudo dnf check-update`    |
| Install a package        | `sudo apt install nginx` | `sudo dnf install nginx`   |
| Upgrade packages         | `sudo apt upgrade`       | `sudo dnf upgrade`         |
| Search packages          | `apt search nginx`       | `dnf search nginx`         |

Some Amazon Linux releases use `yum`; check the distribution before following commands from a tutorial.

## 10. SSH and file transfer

SSH provides secure remote terminal access.

```
ssh -i my-key.pem ubuntu@SERVER_IP
```

To copy a file to a remote server:

```
scp -i my-key.pem app.jar ubuntu@SERVER_IP:/home/ubuntu/
```

To copy a directory:

```
scp -i my-key.pem -r ./config ubuntu@SERVER_IP:/home/ubuntu/
```

For production environments, prefer managed access or tightly restricted SSH access, protect private keys, and avoid sharing credentials.

## 11. Linux shell scripting

A shell script automates commands that would otherwise be executed manually.

Create a file named `deploy.sh`:

```
#!/bin/bash
set -e

echo "Starting deployment..."

if command -v java >/dev/null 2>&1; then
    java -version
else
    echo "Java is not installed"
    exit 1
fi

echo "Deployment checks completed."
```

Run it:

```
chmod +x deploy.sh
./deploy.sh
```

Important concepts:

- `#!/bin/bash`: Specifies the interpreter.
- `echo`: Prints messages.
- `if`: Performs conditional checks.
- `exit 1`: Exits with a nonzero status to indicate failure.
- `set -e`: Stops the script when certain commands fail.
- `$1`, `$2`: Access positional arguments.
- `$?`: Contains the exit status of the previous command.

Example:

```
#!/bin/bash
echo "Application: $1"
echo "Environment: $2"
```

Execute with:

```
./deploy.sh student-app production
```

## 12. Environment variables

Environment variables provide configuration to applications without hardcoding values in the application source.

```
export APP_ENV=production
echo "$APP_ENV"
printenv
```

For a Spring Boot application:

```
export SPRING_PROFILES_ACTIVE=prod
export DB_HOST=my-database.example
java -jar app.jar
```

These are example settings, not actual database credentials.

Security rule: Do not commit passwords, database credentials, or secret API keys to GitHub. Use an appropriate secret manager, protected CI/CD credentials, or securely configured environment variables. Remember that environment variables may be visible to privileged users and processes.

## 13. Scheduling jobs with cron

Cron runs scheduled commands on supported Linux systems.

Edit your user's crontab:

```
crontab -e
```

Example schedule:

```
0 2 * * * /home/ubuntu/backup.sh
```

This runs the script every day at 2:00 AM according to the system's configured time zone.

Cron syntax:

`minute hour day-of-month month day-of-week`

Examples:

| Schedule      | Meaning              |
| ------------- | -------------------- |
| `0 * * * *`   | Every hour           |
| `*/5 * * * *` | Every five minutes   |
| `0 2 * * *`   | Daily at 2 AM        |
| `0 9 * * 1`   | Every Monday at 9 AM |

Check existing schedules using `crontab -l`.

## 14. Important Linux troubleshooting scenarios

Scenario 1: Jenkins service is not running

```
sudo systemctl status jenkins
sudo journalctl -u jenkins -n 100
sudo systemctl restart jenkins
```

Check the logs for Java problems, configuration errors, or port conflicts before restarting repeatedly.

Scenario 2: Docker permission denied

```
whoami
id
docker ps
sudo docker ps
```

If `sudo docker ps` works but `docker ps` fails, investigate Docker socket permissions and group membership. Re-login after group changes, and remember that Docker group access is highly privileged.

Scenario 3: Application container exited

```
docker ps -a
docker logs --tail 100 backend-container
docker inspect backend-container
```

Check the application exception, environment variables, database connectivity, port configuration, and container exit code.

Scenario 4: Port 8080 is not accessible

```
sudo ss -ltnp
curl -v http://localhost:8080
```

If the local request fails, inspect the application process and logs. If local access works but external access fails, check port binding, firewall rules, AWS security groups, and network routes.

Scenario 5: Permission denied for a file

```
ls -l file.txt
namei -l /path/to/file.txt
id
```

Inspect the file's ownership and permissions, along with execute permissions on parent directories. Change only the permissions or ownership that actually need correction.

## 15. Common Linux interview questions

1\. What is the difference between Linux and Unix?Unix refers to a family of operating systems and related systems. Linux is an open-source Unix-like kernel used by many distributions.

2\. What is the difference between absolute and relative paths?An absolute path starts from the root directory, such as /home/ubuntu/app. A relative path is interpreted from the current directory, such as ./app.

3\. What is the difference between hard and symbolic links?A hard link refers to the same underlying filesystem inode. A symbolic link stores a path to another file or directory and can become broken if its target moves.

4\. What is the difference between a process and a thread?A process is a running program with its own process resources and address space. Threads are execution units within a process that generally share its memory.

5\. What is the difference between kill and kill -9?kill normally sends SIGTERM, allowing graceful shutdown. kill -9 sends SIGKILL, which cannot be caught or ignored and prevents graceful cleanup.

6\. What is the difference between grep and find?grep searches text for matching patterns. find searches the filesystem for files and directories matching specified conditions.

7\. What is the difference between df and du?df reports filesystem space usage. du estimates disk usage for specified files and directories.

8\. What is the root user?Root is the superuser with broad administrative privileges on Linux.

9\. What is a daemon?A daemon is a background process that provides a service or performs tasks, often without direct user interaction.

10\. What does chmod 755 mean?The owner receives read, write, and execute permissions. The group and others receive read and execute permissions.

11\. How do you check a running service?Use systemctl status service-name on systems managed by systemd, then inspect service logs if necessary.

12\. How do you find which process uses a port?Use sudo ss -ltnp for TCP listening sockets, or sudo lsof -i :8080 if lsof is installed.

13\. What is a zombie process?A zombie is a process that has exited but whose parent has not yet collected its exit status. It usually cannot be terminated like a running process.

14\. What is the PATH variable?PATH is a colon-separated list of directories searched by the shell when resolving executable commands.

15\. How do you troubleshoot high CPU usage?Use top or ps to identify processes consuming CPU, then examine their logs and workload before taking corrective action.

----

# AWS Notes for DevOps Interviews and Practical Projects

These notes cover AWS fundamentals, important services, practical commands, Jenkins integration, and common interview questions. They are designed for revision and hands-on preparation.

## 1. What is AWS?

AWS (Amazon Web Services) is a cloud computing platform that provides on-demand services such as servers, storage, databases, networking, security, and monitoring.

Instead of purchasing physical servers, you can launch virtual servers, store files, host applications, and manage databases over the internet.

Interview answer: “AWS is a cloud platform that provides scalable, secure, and on-demand IT resources. In DevOps, we use AWS to host applications, automate deployments, manage databases and storage, and monitor infrastructure.”

## 2. Important AWS services

[Icon AWS to EC2 Service | Free SVG](https://images.openai.com/static-rsc-4/HP4rSceyP5-tzH7DCxzlWC6Pa8km-RSqNN5paDGvQ-lZ7LRHknP_WmDTLVu85tFg3wdQYjuL2wSPYsDSHvzdhdv6-CZG_tzRoZyxnbbdxbhzv0n_1-UBJYJc975SbKXyviIE_bs3vqFKgMqKB3C8GhGMztpuSFxp5XdpkKu3ymY?purpose=inline)

Amazon EC2 — Virtual servers

Launch Linux or Windows servers to host applications, Jenkins, Docker containers, and web servers.

Example: Run Jenkins on one EC2 instance and deploy your Spring Boot application to another.

[Amazon S3 monitoring & observability | Dynatrace Hub](https://images.openai.com/static-rsc-4/VlRJOCPLELSpJ3rtJaSuyp83dcy4sXvgplJhMmu3qKno1hxB3VDp91vvnY-C8hHE4x0v6GBO8toZJYyZJ-VgN2cS1mLnNb_yatWok2LyQtkN9t64dY2ailARVeVyNLrWzsG0udQnfRsmpcmvxtSsWEXwAzIUEMATxgaCJmpr_MQ?purpose=inline)

Amazon S3 — Object storage

Store images, documents, backups, logs, and static website files in buckets.

Example: Store student application uploads in an S3 bucket.

[Database | Devops Spundan](https://images.openai.com/static-rsc-4/ru_fUoYKe5y96DQz8X0V5GjPhnrc7rLmZmcj6WmnRgdqnWDTf4mDqUmGJek2fWOfZ1Dm-9NUqhQlbs9z_2YTOexDMeD00B3YHYp4Ym604_iZyXnaun-mkiTUS7Buw_TVixA0MqBnVypjHZ3EZdzUTBqsuqtmRUT4xuWtDYPl-oA?purpose=inline)

Amazon RDS — Managed databases

Run relational databases such as MySQL, PostgreSQL, and other supported database engines without managing the underlying database server hardware.

Example: Connect a Spring Boot backend running on EC2 to a PostgreSQL RDS database.

[Set up a Virtual Private Cloud with AWS VPC](https://images.openai.com/static-rsc-4/bBcn9Bu2wAX-ReJwP9vTSwYXLQdVPk1u5IWJ3I5i5EXHg1yJfOaC-elavYGv6URRscb0GTrryl3yoStHlent455xJiih_rOuDUpzmnalYa-6w3UkiTVwzLfR5IRhCgAhe1mckh9fZDPVrbbwNiFPU0NKlU3n4zIe6IFU2OblgMw?purpose=inline)

Amazon VPC — Networking

Create an isolated virtual network with subnets, route tables, IP ranges, and network security controls.

[Optimize AWS IAM Security & Access Control](https://images.openai.com/static-rsc-4/qaN4N-TFV3Ndu3ffwH_2f93--anKdGJEEuuvUbFVDQtlYeAuLsh91gCyb0R5L4DDFz_tMATrSTmRoXybw4fpaqbja9TgeZOtki7J6bfvz4-QdKEKoLZ9poZZf1zpfTo8v3Dd-ASeTczYVnMORqL4yqhhDBl2K1Wq8CFOH8GOATI?purpose=inline)

AWS IAM — Identity and access management

Manage users, roles, policies, and permissions. Follow least privilege: give each identity only the access it needs.

[Amazon CloudWatchとは？機能や特徴・製品の概要まとめ](https://images.openai.com/static-rsc-4/riPShc1DOTdBztTDFN9nJNoD1ZOuRXn6MqpaxbnVFxE4YsjPnDkWK7x5ZTwLmAwQqdYb014V5gXMKvF2AapRCum0-acwoZGmN868EaZB5bCKRIqCdyBpS0waqn_VG0DCgB4xsHVVLGkgP2Y1QP9uLV9Sh8YpbhxVCiu0M_3LNag?purpose=inline)

Amazon CloudWatch — Monitoring

Collect metrics and logs, create dashboards, and configure alarms for supported AWS resources and applications.

Other important services:

| Service         | Purpose                                                         |
| --------------- | --------------------------------------------------------------- |
| EBS             | Persistent block storage for EC2                                |
| ELB / ALB       | Distribute incoming traffic across targets                      |
| Auto Scaling    | Adjust the number of EC2 instances based on configured policies |
| Route 53        | DNS and domain routing                                          |
| ECR             | Store Docker container images                                   |
| ECS             | Run and manage containers                                       |
| EKS             | Managed Kubernetes                                              |
| Lambda          | Run code without managing servers                               |
| CloudTrail      | Record AWS API activity for auditing                            |
| Secrets Manager | Store and manage secrets                                        |
| Systems Manager | Manage and access supported servers and AWS resources           |
| CloudFormation  | Provision AWS infrastructure using templates                    |

## 3. AWS global infrastructure

AWS Region

A geographic area, such as Mumbai (`ap-south-1`)

Availability Zone A

Independent infrastructure location

Availability Zone B

Separate location for resilience

- Region: A geographic area containing multiple Availability Zones.
- Availability Zone (AZ): One or more discrete data centers with independent infrastructure.
- Edge location: A location used by services such as CloudFront to deliver content closer to users.

Interview question: Why use multiple Availability Zones?

To improve availability and resilience. If an application component in one AZ fails, appropriately designed infrastructure in another AZ can continue serving traffic.

## 4. Amazon EC2 — practical notes

EC2 provides virtual machines in the AWS cloud.

Important concepts:

- AMI: Template used to launch an instance.
- Instance type: Determines compute resources, including CPU and memory.
- Key pair: Used for supported secure login methods, such as SSH.
- Security group: Virtual firewall controlling inbound and outbound traffic.
- EBS volume: Persistent block storage for an instance.
- Elastic IP: Static public IPv4 address that can be associated with supported resources.
- User data: Startup instructions that can run when an instance launches.

### Common ports

| Port | Purpose                                                      |
| ---- | ------------------------------------------------------------ |
| 22   | SSH                                                          |
| 80   | HTTP                                                         |
| 443  | HTTPS                                                        |
| 8080 | Common application or Jenkins port; depends on configuration |
| 3306 | MySQL                                                        |
| 5432 | PostgreSQL                                                   |

Only open the ports your application needs. For SSH, restrict access to your own IP or use a managed access method such as Systems Manager where suitable.

### Connect to an EC2 Linux instance

From a computer with your downloaded private key:

```
chmod 400 my-key.pem

ssh -i my-key.pem ec2-user@PUBLIC_IP
```

For Ubuntu AMIs, the default username is commonly `ubuntu`; for Amazon Linux, it is commonly `ec2-user`.

Replace `PUBLIC_IP` with the instance's actual public IP address. The instance must also have suitable networking and security-group rules.

### Basic Linux checks on EC2

```
whoami
hostname
df -h
free -m
top
```

These commands show your current user, hostname, disk usage, memory usage, and running processes.

## 5. Amazon S3 — storage notes

S3 stores data as objects inside buckets.

Important concepts:

- Bucket: Container for objects.
- Object: A file and its associated metadata.
- Bucket policy: Resource-based permissions for the bucket.
- Versioning: Maintains versions of objects.
- Lifecycle rules: Transition or expire objects based on configured rules.
- Encryption: Protects stored data.
- Block Public Access: Helps prevent unintended public exposure.

### AWS CLI commands

First, configure an appropriate AWS identity and install AWS CLI.

```
aws sts get-caller-identity
aws s3 ls
aws s3 mb s3://YOUR-UNIQUE-BUCKET-NAME
aws s3 cp report.pdf s3://YOUR-UNIQUE-BUCKET-NAME/
aws s3 ls s3://YOUR-UNIQUE-BUCKET-NAME/
```

The bucket name must be globally unique. The commands require appropriate permissions.

For an EC2 application to access S3, prefer an IAM role attached to the instance rather than saving long-lived AWS access keys on the server.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://repost.aws\&sz=32)

AWS re:Post+1



## 6. IAM — security and permissions

IAM controls who can access AWS resources and which actions they can perform.

| Component | Meaning                                                                                    |
| --------- | ------------------------------------------------------------------------------------------ |
| User      | Identity generally associated with a person or application requiring long-term credentials |
| Group     | Collection of IAM users                                                                    |
| Role      | Identity with permissions that trusted users or services can assume                        |
| Policy    | Document defining allowed or denied actions                                                |
| MFA       | Additional authentication factor                                                           |

Example: Your EC2 application needs to upload reports to S3. Create a role with only the necessary permissions for the required bucket and attach the role to the EC2 instance.

Avoid embedding AWS access keys in GitHub repositories, Docker images, application code, or Jenkinsfiles.

## 7. VPC — networking notes

A VPC is a logically isolated network for your AWS resources.

Internet

Internet Gateway

### VPC

Public subnet

Internet-facing load balancer or bastion host, when required

Private subnet

Application servers and databases, where appropriate

Key terms:

- Subnet: A range of IP addresses within a VPC, associated with an Availability Zone.
- Route table: Defines where network traffic is routed.
- Internet Gateway: Enables internet connectivity for resources with appropriate routing and public addressing.
- NAT Gateway: Allows supported outbound internet access from private subnets without directly exposing those resources to inbound internet connections.
- Security group: Stateful virtual firewall associated with resources such as EC2 instances.
- Network ACL: Stateless traffic filter applied at the subnet level.

Interview question: What is the difference between a security group and a network ACL?

A security group is stateful and applies to associated resources. A network ACL is stateless and applies at the subnet level, so return traffic must be allowed by its rules as well.

## 8. RDS — managed database

Amazon RDS runs managed relational database engines such as MySQL and PostgreSQL.

Advantages include managed backups, supported patching and maintenance, monitoring, and options for high availability.

For a typical web application:

- EC2 runs the Spring Boot backend.
- RDS stores application records.
- S3 stores uploaded documents and images.
- IAM controls access to AWS services.

Important: Do not expose the RDS database publicly unless there is a specific, justified requirement. Normally, permit database connections from the application's security group only.

## 9. CloudWatch and CloudTrail

| CloudWatch                                        | CloudTrail                                             |
| ------------------------------------------------- | ------------------------------------------------------ |
| Monitors metrics, logs, and alarms                | Records AWS API activity and events                    |
| Helps diagnose performance and application issues | Helps investigate who performed an AWS action and when |
| Example: Alert when CPU utilization stays high    | Example: Investigate who changed an EC2 security group |

CloudWatch can collect metrics and logs and trigger alarms to help teams detect and investigate operational problems.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://aws.amazon.com\&sz=32)

Amazon Web Services+1



## 10. AWS CLI — essential commands

The AWS CLI lets you manage AWS resources from a terminal.

```
# Check the active AWS identity
aws sts get-caller-identity

# Show configured CLI settings
aws configure list

# List EC2 instances in the configured Region
aws ec2 describe-instances

# List S3 buckets
aws s3 ls

# List RDS database instances
aws rds describe-db-instances

# Show the configured default Region
aws configure get region
```

Use `--region ap-south-1` to specify Mumbai explicitly when needed. AWS CLI commands require valid credentials and sufficient permissions.

For applications on EC2, prefer IAM roles over long-lived access keys. For local development, use an appropriate named profile or another supported credential method.

## 11. How AWS works with DevOps tools

GitHub

Source code and Jenkinsfile

Jenkins

Build, test, package, deploy

Amazon ECR

Container image registry

Terraform

Provision infrastructure

Amazon EC2 / ECS / EKS

Run the deployed application

Amazon RDS

Database

Amazon S3

Object storage

This is a conceptual architecture. A project does not need every service: for example, you can deploy a Dockerized Spring Boot application directly to EC2 without ECR, ECS, or EKS.

### Example deployment workflow

1. Developer pushes Spring Boot code to GitHub.
2. GitHub triggers Jenkins using a webhook.
3. Jenkins builds and tests the application.
4. Jenkins builds a Docker image.
5. The image is pushed to ECR, or another configured registry.
6. Jenkins deploys the image to EC2 or an appropriate container service.
7. The application connects to RDS and uses S3 if required.
8. CloudWatch collects relevant logs and metrics.

## 12. AWS interview questions and answers

1\. What is cloud computing?Cloud computing provides on-demand access to computing resources over a network, typically with usage-based pricing.

2\. What is the difference between EC2 and S3?EC2 provides virtual compute capacity. S3 provides scalable object storage for files and data.

3\. What is an AMI?An Amazon Machine Image is a template used to launch EC2 instances, containing information such as the operating system and configured software.

4\. What is Auto Scaling?Auto Scaling adjusts the number of resources according to configured conditions and desired capacity, helping meet demand and improve resilience.

5\. What is an Elastic Load Balancer?It distributes incoming traffic across configured targets, such as EC2 instances, according to the load balancer type and configuration.

6\. What is the difference between IAM user and IAM role?A user is an identity that may have long-term credentials. A role provides temporary credentials when assumed by an authorized identity or service.

7\. What is a VPC?A VPC is a logically isolated virtual network where we configure IP ranges, subnets, routing, and network security.

8\. What is the difference between EBS and S3?EBS provides block storage for EC2 workloads. S3 stores objects and is accessed through APIs or tools.

9\. How do you secure an EC2 instance?Restrict security-group rules, use IAM roles, patch the operating system, protect credentials, use secure access methods, and monitor logs.

10\. How does Jenkins access AWS?Use an IAM role where supported, or securely configured temporary or stored credentials with least-privilege permissions. Avoid hardcoding access keys.

11\. What happens if an EC2 instance stops?Compute billing generally stops for a stopped instance, but attached EBS storage and other retained resources can still incur charges. Public IPv4 address behavior depends on how it is assigned.

12\. What is the difference between CloudWatch and CloudTrail?CloudWatch focuses on monitoring metrics, logs, and alarms. CloudTrail records AWS API activity for auditing and investigation.

