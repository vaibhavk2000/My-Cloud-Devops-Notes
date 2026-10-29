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

----
# Git & GitHub Notes for DevOps Interviews
## Complete Revision Guide: Beginner to Practical Level

These notes cover Git fundamentals, essential commands, branching, merging, conflict resolution, GitHub, Jenkins integration, and real-world troubleshooting.

The goal is to help you **understand Git, explain it confidently in interviews, and use it in practical DevOps projects**.

## 1. What Is Git?

**Definition:** Git is a distributed version control system used to track changes in source code and other files.

It allows developers to maintain a history of changes, collaborate with other developers, create branches, and restore earlier versions when necessary.

### Why do we use Git?

Suppose three developers are working on the same application. Without version control, managing changes and identifying who modified a file can become difficult.

Git helps developers:
- Track changes to files.
- Maintain a history of commits.
- Work on separate branches.
- Merge changes from multiple developers.
- Compare different versions of code.
- Revert unwanted changes.
- Collaborate through remote repositories.

### Interview-ready answer

Git is a distributed version control system used to track and manage changes in source code.

It allows developers to maintain a history of changes, create branches for new features, collaborate with team members, and revert unwanted modifications when necessary.

In DevOps, Git is commonly used to manage application code and infrastructure configuration. For example, we can store application code and Jenkins pipeline files in GitHub, and Jenkins can retrieve the latest code to start a CI/CD pipeline.

**Remember these keywords:** Version control, distributed system, repository, commit, branch, merge, collaboration.

---

## 2. What Is the Difference Between Git and GitHub?

This is one of the most frequently asked Git interview questions.

| Git | GitHub |
|---|---|
| A version control system | An online platform for hosting Git repositories and collaborating |
| Installed and used locally or through supported services | Commonly accessed through a website, APIs, and Git clients |
| Tracks changes and creates commits | Hosts repositories and supports pull requests, reviews, and collaboration |
| Can work without an internet connection for many operations | Usually requires network access for remote collaboration |
| Provides commands such as `git commit` and `git merge` | Provides features such as pull requests, issues, and repository permissions |

### Interview-ready answer

Git is a distributed version control system that tracks changes in files and source code.

GitHub is a platform that hosts Git repositories and provides collaboration features such as pull requests, code reviews, and issue tracking.

For example, I can use Git on my local machine to commit application code and push it to a GitHub repository. Other developers can then review the code and collaborate on the same project.

**Important:** Git can work independently of GitHub. You can also host Git repositories on other platforms or your own server.

---

# 3. Git Architecture

To understand Git, learn its three main local areas and the remote repository.

| Area | Meaning |
|---|---|
| Working directory | Files you currently work on |
| Staging area | Changes selected for the next commit |
| Local repository | The local history of commits |
| Remote repository | A repository hosted elsewhere, such as on GitHub |

### How Git works



Typical workflow:

```text
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Repository
       |
       | git push
       v
Remote Repository (GitHub)
```

### Example

You modify `app.java`.

First, check the changes:

```bash
git status
```

Stage the file:

```bash
git add app.java
```

Commit the staged change:

```bash
git commit -m "Update application logic"
```

Push the commit to GitHub:

```bash
git push origin main
```

**Interview question: What is the staging area in Git?**

The staging area is where we select the changes that should be included in the next commit. It allows us to commit some changes without necessarily committing every modified file.

---

# 4. Git Installation and Initial Configuration

Check whether Git is installed:

```bash
git --version
```

On Ubuntu, install Git with:

```bash
sudo apt update
sudo apt install git -y
```

Configure your name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Verify the configuration:

```bash
git config --global --list
```

**Important:** The configured Git name and email are recorded in commits. Use the identity appropriate for your work.

---

# 5. Essential Git Commands

These are the commands you should know for practical work and interviews.

## A. Repository commands

| Command | Purpose |
|---|---|
| `git init` | Initializes a Git repository |
| `git clone <url>` | Copies an existing repository |
| `git status` | Shows the current working-tree status |
| `git add <file>` | Stages changes to a file |
| `git add .` | Stages eligible changes under the current directory |
| `git commit -m "message"` | Creates a commit from staged changes |
| `git log` | Shows commit history |
| `git diff` | Shows unstaged changes |
| `git diff --staged` | Shows staged changes |

### Example: Create a new repository

```bash
mkdir student-app
cd student-app

git init

echo "# Student App" > README.md

git status
git add README.md
git commit -m "Initial commit"
```

This creates a repository and records the README file in its history.

## B. Remote repository commands

| Command | Purpose |
|---|---|
| `git remote -v` | Lists configured remotes |
| `git remote add origin <url>` | Adds a remote named `origin` |
| `git fetch origin` | Downloads remote references and objects without integrating them into your current branch |
| `git pull` | Fetches changes and integrates them into the current branch according to the configured strategy |
| `git push` | Sends local commits to a remote repository |
| `git ls-remote origin` | Lists references available on the remote |

### Example: Push a local repository to GitHub

Create an empty repository on GitHub, then configure it as the remote:

```bash
git branch -M main

git remote add origin https://github.com/USERNAME/REPOSITORY.git

git push -u origin main
```

Replace the placeholders with your actual repository details.

The `-u` option sets the upstream branch so future pushes and pulls can use the configured tracking relationship.

**Security tip:** Never commit passwords, AWS credentials, API keys, private keys, or other secrets to GitHub.

---

# 6. What Is a Git Branch?

A branch allows you to develop a feature or fix a problem separately from another line of development.

For example, suppose your application has a `main` branch. You want to develop a login feature without making changes directly to `main`.

You can create a feature branch:

```bash
git branch
```

Lists local branches.

```bash
git branch feature-login
```

Creates a branch.

```bash
git switch feature-login
```

Switches to the branch.

You can also create and switch in one command:

```bash
git switch -c feature-login
```

### Other useful branch commands

```bash
git branch -a
```

Lists local branches and remote-tracking branches.

```bash
git branch -d feature-login
```

Deletes a local branch if Git considers it safe to delete.

```bash
git branch -m old-name new-name
```

Renames a branch.

### Interview question: Why do we use branches in Git?

**Answer:**

Branches allow developers to work on features, bug fixes, or experiments independently of the main development line. Once the work is reviewed and tested, the changes can be merged into the target branch.

This helps teams collaborate without requiring every developer to modify the same branch directly.

---

# 7. Git Merge vs. Git Rebase

Both operations can integrate changes from different lines of development, but they work differently.

## Git merge

Merge combines changes from different branches. When necessary, it creates a merge commit that records the integration.

Example:

```bash
git switch main
git merge feature-login
```

This integrates `feature-login` into `main`.

## Git rebase

Rebase reapplies commits onto a different base commit, creating new commit IDs for the reapplied commits.

Example:

```bash
git switch feature-login
git rebase main
```

This reapplies the feature branch's commits on top of the current `main` branch.

### Comparison

| Git merge | Git rebase |
|---|---|
| Integrates histories | Reapplies commits onto a new base |
| Can preserve the original branch structure | Produces a more linear history in many workflows |
| May create a merge commit | Rewrites the commits being rebased |
| Usually preserves existing commit IDs | Creates new commit IDs for reapplied commits |

**Important:** Avoid rebasing shared commits that other developers rely on unless the team has agreed on how to handle the rewritten history.

### Interview-ready answer

Git merge integrates changes from one branch into another while preserving the existing branch history. Depending on the situation, Git may create a merge commit.

Git rebase reapplies commits onto a new base, which can create a cleaner, more linear history.

I would use merge when preserving the branch integration history is important. I would use rebase to update my own feature branch with the latest changes when appropriate.

I would be careful when rebasing shared branches because rebase changes commit history.

---

# 8. What Is a Pull Request?

A pull request (PR) is a collaboration feature offered by platforms such as GitHub. It allows developers to propose changes from one branch for review and integration into another branch.

A typical workflow is:

```text
Create Feature Branch
        |
        v
Make Code Changes
        |
        v
Commit Changes
        |
        v
Push Feature Branch
        |
        v
Create Pull Request
        |
        v
Code Review and Tests
        |
        v
Merge into Main
```

### Example commands

```bash
git switch -c feature-login

# Make changes to your files.

git add .
git commit -m "Add login feature"

git push -u origin feature-login
```

You can then open a pull request on GitHub.

### Interview question: What is the difference between `git pull` and a pull request?

- `git pull` is a Git command that fetches and integrates remote changes into the current branch.
- A pull request is a platform-based workflow for reviewing and proposing changes before merging them into another branch.

**Remember:** These are not the same thing.

---

# 9. What Is a Merge Conflict?

A merge conflict occurs when Git cannot automatically reconcile changes, such as when two branches make incompatible edits to the same part of a file.

### Example

Suppose `main` contains:

```java
String message = "Hello";
```

Your feature branch changes it to:

```java
String message = "Welcome";
```

Meanwhile, another developer changes the same line on `main` to:

```java
String message = "Good morning";
```

When you try to merge the branches, Git may report a conflict.

### Conflict markers

Git may display:

```text
<<<<<<< HEAD
String message = "Good morning";
=======
String message = "Welcome";
>>>>>>> feature-login
```

These markers indicate the conflicting versions.

### How to resolve a conflict

1. Open the conflicted file.
2. Review both versions.
3. Decide which changes to keep, or combine them.
4. Remove the conflict markers.
5. Test the corrected file.
6. Stage the resolved file.
7. Complete the merge.

Commands:

```bash
git status
```

After editing the file:

```bash
git add app.java
git commit
```

For a merge conflict, Git typically allows you to complete the merge with a commit after staging the resolution. If you are rebasing, the next step is generally `git rebase --continue` instead.

### Interview-ready answer

A merge conflict occurs when Git cannot automatically combine changes from different branches.

For example, two developers might modify the same line in the same file. Git then requires us to resolve the conflicting changes manually.

I first check the conflict using git status, open the affected files, compare both versions, and decide which changes to keep. After resolving the conflict, I stage the files, complete the merge, and run the required tests to verify that the application still works.

---

# 10. Git Reset vs. Git Revert

This is a very important interview question because both commands can undo changes, but they behave differently.

## A. Git reset

`git reset` moves the current branch reference to another commit and can also change the staging area or working tree, depending on the options used.

Common types:

| Command | Effect |
|---|---|
| `git reset --soft HEAD~1` | Moves the branch back one commit but keeps the changes staged |
| `git reset --mixed HEAD~1` | Moves the branch back one commit and unstages the changes, generally keeping the file changes in the working tree |
| `git reset --hard HEAD~1` | Moves the branch back one commit and resets the index and working tree, discarding affected uncommitted changes |

**Warning:** `git reset --hard` can permanently discard uncommitted work. Use it only when you understand which changes will be lost.

## B. Git revert

`git revert` creates a new commit that reverses the changes introduced by an earlier commit.

Example:

```bash
git revert <commit-id>
```

This is often safer for undoing changes on a shared branch because it preserves the existing commit history and records the reversal.

### Comparison

| Git reset | Git revert |
|---|---|
| Moves the branch reference | Creates a new commit to reverse changes |
| Can rewrite branch history | Preserves existing commits |
| Commonly used for local cleanup | Commonly used to undo changes on shared branches |
| Can discard changes depending on options | Usually preserves the original history and records the reversal |

### Interview-ready answer

Git reset moves the current branch to an earlier commit and can also modify the staging area and working tree, depending on the option used.

Git revert creates a new commit that reverses the changes introduced by an earlier commit.

I generally prefer git revert when undoing an unwanted commit on a shared branch because it preserves the existing history. I use git reset when I need to adjust local history and understand the impact of that operation.

---

# 11. What Is Git Stash?

`git stash` temporarily saves eligible uncommitted changes so you can switch tasks without committing unfinished work.

### Example scenario

You are working on a new feature when your team asks you to fix an urgent production bug.

You don't want to commit your unfinished feature yet.

Save the changes:

```bash
git stash
```

List saved stashes:

```bash
git stash list
```

Restore the most recent stash and remove it from the stash list:

```bash
git stash pop
```

Restore the most recent stash without removing it:

```bash
git stash apply
```

Include untracked files in a stash:

```bash
git stash push -u -m "Work in progress"
```

### Interview answer

Git stash temporarily saves eligible uncommitted changes, allowing me to switch branches or handle another task without creating an unfinished commit. I can later restore those changes using `git stash pop` or `git stash apply`.

---

# 12. What Is Git Cherry-Pick?

`git cherry-pick` applies the changes introduced by a specific commit onto the current branch, creating a new commit.

### Example scenario

A bug fix has been committed to a development branch, but you need that fix in another branch without merging all the development changes.

You can cherry-pick the specific commit.

```bash
git switch main
git cherry-pick <commit-id>
```

### Interview answer

Git cherry-pick allows me to apply a specific commit from another branch to my current branch. It is useful when I need an isolated bug fix without merging the entire source branch.

**Important:** Cherry-picking creates a new commit with a different commit ID. It should be used thoughtfully to avoid duplicating changes or complicating branch history.

---

# 13. What Is `.gitignore`?

A `.gitignore` file specifies untracked files and directories that Git should ignore.

For example, a Java project might use:

```gitignore
target/
*.log
.env
.idea/
.vscode/
```

This tells Git to ignore matching build output, log files, environment files, and IDE configuration directories.

### Why is `.gitignore` important in DevOps?

It helps prevent unnecessary files, local configuration, build artifacts, and accidentally exposed secrets from being added to the repository.

However, **adding a file to `.gitignore` does not automatically stop tracking it if it has already been committed**.

To stop tracking an already tracked file while retaining the local copy:

```bash
git rm --cached .env
git commit -m "Stop tracking environment file"
```

Do this only after confirming the file should be removed from version control. If a secret was already committed or pushed, removing it from the latest commit does not make the exposed secret safe again. Rotate the secret and follow the appropriate repository cleanup procedure.

### Interview question: Why should we not upload `.env` files to GitHub?

An `.env` file may contain database passwords, API keys, or other sensitive configuration. Committing it can expose those secrets to unauthorized users.

Instead, keep local environment files out of version control and use a suitable secrets-management mechanism in deployment environments.

---

# 14. What Is the Difference Between `git fetch` and `git pull`?

| `git fetch` | `git pull` |
|---|---|
| Downloads remote objects and updates remote-tracking references | Fetches remote changes and integrates them into the current branch |
| Does not automatically merge changes into the current branch | Usually integrates changes using merge or rebase, depending on configuration |
| Useful for inspecting changes before integrating them | Useful when you are ready to update your current branch |

### Example

Fetch remote changes:

```bash
git fetch origin
```

Inspect the difference between your current branch and the remote-tracking branch:

```bash
git log HEAD..origin/main --oneline
```

Integrate remote changes:

```bash
git pull origin main
```

The pull operation can produce conflicts if local and remote changes cannot be integrated automatically.

### Interview-ready answer

Git fetch downloads updates from a remote repository without integrating them into my current branch. Git pull fetches those updates and then integrates them according to the configured strategy.

I use fetch when I want to inspect remote changes before deciding how to integrate them.

---

# 15. What Is HEAD in Git?

`HEAD` is a reference that identifies the current checkout, usually the commit associated with the currently checked-out branch.

For example:

```bash
git log --oneline
```

Shows recent commits.

```bash
git show HEAD
```

Displays information about the commit referenced by `HEAD`.

```bash
git diff HEAD
```

Shows differences between the working tree and the commit referenced by `HEAD`, including relevant staged and unstaged changes.

### What does `HEAD~1` mean?

`HEAD~1` refers to the first ancestor of the current commit, following the first-parent chain.

For example:

```bash
git show HEAD~1
```

Displays the previous commit along that chain.

### Interview answer

HEAD identifies the current checkout in Git, usually the latest commit of the currently checked-out branch. We can use references such as `HEAD` and `HEAD~1` to inspect commits or perform history operations.

---

# 16. What Is a Git Tag?

A Git tag gives a name to a specific commit, commonly to identify a release.

For example, you might tag a tested application release as `v1.0.0`.

### Important commands

Create a lightweight tag:

```bash
git tag v1.0.0
```

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

List tags:

```bash
git tag
```

Push a particular tag to GitHub:

```bash
git push origin v1.0.0
```

### Interview answer

Git tags identify specific commits, often representing software releases. For example, we can tag a stable application version as `v1.0.0` and use that release identifier in our deployment process.

**DevOps connection:** Release tags can help teams associate a deployed application version with a specific source-code commit.

---

# 17. Git in Jenkins CI/CD Pipelines

Git is a fundamental component of most CI/CD workflows.

### Typical workflow

```text
Developer
    |
    v
Git Repository on GitHub
    |
    v
Webhook or Scheduled Trigger
    |
    v
Jenkins Pipeline
    |
    v
Checkout Source Code
    |
    v
Build and Test
    |
    v
Build Docker Image
    |
    v
Deploy Application
```

### What happens when a developer pushes code?

1. The developer commits changes locally.
2. The developer pushes the commits to GitHub.
3. GitHub can notify Jenkins through a configured webhook.
4. Jenkins starts the configured pipeline.
5. Jenkins checks out the required branch or commit.
6. The pipeline builds and tests the application.
7. Subsequent stages can build a Docker image and deploy the application.

A webhook is one possible trigger; Jenkins can also use polling or other configured triggers.

### Example Jenkinsfile

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/USERNAME/REPOSITORY.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```

This example assumes a Maven project and the necessary tools are installed on the Jenkins agent. It retrieves code and runs the build and tests.

### Interview question: How do you integrate Git with Jenkins?

**Answer:**

I configure Jenkins to access the Git repository and check out the required branch. When code changes are pushed, Jenkins can trigger the pipeline through a webhook or another configured mechanism.

The pipeline then retrieves the source code, builds the application, runs tests, and continues with the required packaging and deployment stages.

For private repositories, I configure appropriate repository credentials in Jenkins rather than embedding credentials in the Jenkinsfile.

---

# 18. Git Troubleshooting Scenarios

Interviewers often ask practical questions to assess whether you can handle real development problems.

## Scenario 1: You committed to the wrong branch

**Situation:** You accidentally committed changes to `main` instead of your feature branch.

First, check the status:

```bash
git status
git log --oneline -5
```

If the commit has not been pushed and you have no conflicting changes, you can create a feature branch at the current commit:

```bash
git switch -c feature-login
```

This creates a new branch containing the commit. If the changes should not remain on `main`, you can then carefully restore `main` to its intended commit, provided doing so will not discard or rewrite shared work.

**Interview tip:** Always ask whether the commit has been pushed before deciding how to repair branch history.

## Scenario 2: You pushed incorrect code to GitHub

A safe approach depends on whether the change is shared and whether rewriting history is acceptable.

If the change is already on a shared branch, you can usually undo it with:

```bash
git revert <commit-id>
git push origin main
```

Review the generated reversal and run appropriate tests before pushing.

## Scenario 3: You cannot push to GitHub

Check the remote:

```bash
git remote -v
```

Check the current branch:

```bash
git branch --show-current
```

Check your repository permissions and authentication method.

Common causes include:
- Incorrect repository URL.
- Missing repository permissions.
- Invalid or expired credentials.
- SSH key configuration problems.
- A branch protection rule rejecting the push.
- Local history that cannot be fast-forwarded to the remote branch.

## Scenario 4: You accidentally staged the wrong file

To unstage a file without discarding its changes:

```bash
git restore --staged filename
```

This removes the file from the staging area but leaves its working-tree modifications intact.

## Scenario 5: You want to discard uncommitted changes

To discard unstaged changes in a specific tracked file:

```bash
git restore filename
```

To unstage changes:

```bash
git restore --staged filename
```

**Warning:** Restoring a file can discard modifications. Inspect `git status` and `git diff` before running commands that might remove work.

---

# 19. Git vs. SVN

Another possible interview question is the difference between Git and SVN.

| Feature | Git | SVN |
|---|---|---|
| Type | Distributed version control | Centralized version control |
| Repository | Each clone contains repository history | A central repository stores the main history |
| Offline work | Many operations work offline | Many operations depend on server access |
| Branching | Lightweight and commonly used | Supported, but traditionally organized differently |
| Collaboration | Developers exchange commits between repositories | Developers commonly interact with a central server |

**Interview answer:** Git is distributed, meaning developers can maintain complete repository history in their local clones. SVN is centralized, meaning the central server plays a more central role in repository history and collaboration.

---

# 20. Top 25 Git Interview Questions

Use this table for quick revision before your interview.

| No. | Interview question | Key point |
|---:|---|---|
| 1 | What is Git? | Distributed version control |
| 2 | What is GitHub? | Git repository hosting and collaboration |
| 3 | Git vs. GitHub? | Tool vs. hosting platform |
| 4 | What is a repository? | A project and its Git history |
| 5 | What is a commit? | A recorded change in repository history |
| 6 | What is a branch? | An independent line of development |
| 7 | What is the staging area? | Changes prepared for the next commit |
| 8 | What is `git clone`? | Copies an existing repository |
| 9 | What is `git pull`? | Fetches and integrates remote changes |
| 10 | What is `git fetch`? | Downloads remote updates without integrating them into the current branch |
| 11 | What is `git push`? | Sends commits to a remote repository |
| 12 | What is `git merge`? | Integrates branch changes |
| 13 | What is `git rebase`? | Reapplies commits onto a new base |
| 14 | What is a merge conflict? | Changes Git cannot automatically reconcile |
| 15 | What is `git stash`? | Temporarily saves eligible uncommitted changes |
| 16 | What is `git cherry-pick`? | Applies a specific commit to the current branch |
| 17 | What is `.gitignore`? | Specifies untracked files Git should ignore |
| 18 | What is `git reset`? | Moves the branch reference and optionally resets the index or working tree |
| 19 | What is `git revert`? | Creates a commit that reverses earlier changes |
| 20 | What is HEAD? | Reference to the current checkout |
| 21 | What is a tag? | A named reference to a specific commit |
| 22 | What is a pull request? | A workflow for proposing and reviewing changes |
| 23 | What is a remote repository? | A repository hosted elsewhere |
| 24 | How do you resolve merge conflicts? | Inspect, resolve, stage, and complete integration |
| 25 | How is Git used in Jenkins? | Source checkout and CI/CD automation |

---

# 21. Quick Git Command Cheat Sheet

Save these commands for your daily revision.

| Task | Command |
|---|---|
| Check Git version | `git --version` |
| Initialize repository | `git init` |
| Clone repository | `git clone <url>` |
| Check status | `git status` |
| Stage a file | `git add <file>` |
| Commit changes | `git commit -m "message"` |
| View history | `git log --oneline` |
| View changes | `git diff` |
| Create a branch | `git branch <name>` |
| Switch branches | `git switch <name>` |
| Create and switch branch | `git switch -c <name>` |
| Merge a branch | `git merge <name>` |
| Update from remote | `git pull` |
| Fetch remote changes | `git fetch` |
| Push commits | `git push` |
| List branches | `git branch -a` |
| List remotes | `git remote -v` |
| Stash changes | `git stash` |
| Restore a stash | `git stash pop` |
| Undo a commit safely on a shared branch | `git revert <commit-id>` |
| Unstage a file | `git restore --staged <file>` |
| Inspect a commit | `git show <commit-id>` |

---

# 22. Explain Git in 60 Seconds

Practice this answer aloud until you can explain it naturally.

Git is a distributed version control system used to track changes in source code and collaborate with other developers.

It allows us to create commits, maintain code history, work on different branches, merge changes, and resolve conflicts.

GitHub is a platform where we can host Git repositories and collaborate through pull requests and code reviews.

In DevOps, Git is commonly integrated with Jenkins to automate CI/CD pipelines. When developers push code to GitHub, Jenkins can retrieve the latest changes, build the application, run tests, and proceed with Docker image creation and deployment.

Git helps teams maintain version history, collaborate effectively, and manage application changes in a controlled way.

---

# Docker Notes for DevOps Interviews 🐳
## Complete Revision Guide: Beginner to Interview Level

These notes will help you understand Docker, explain concepts confidently in interviews, practice commands, and troubleshoot real-world issues.

We'll use practical examples from your DevOps projects, including Jenkins pipelines, Spring Boot applications, Docker containers, and AWS EC2 deployments.

## 1. What Is Docker?

**Definition:** Docker is a platform that allows us to build, package, distribute, and run applications inside containers.

A container packages an application with its required libraries and dependencies so it can run consistently across supported environments.

### Interview-ready answer

Docker is a containerization platform that allows us to package an application along with its dependencies into a container.

It helps ensure that applications run consistently across different environments, such as development, testing, and production.

In DevOps, Docker is commonly used to simplify application deployment, improve portability, and make it easier to scale and manage applications.

For example, we can containerize a Spring Boot application using a Dockerfile, build a Docker image, and run that image as a container on an AWS EC2 instance.

### Why do we use Docker?

Without Docker, an application might work on a developer's laptop but fail on a server because of differences in software versions, libraries, or configuration.

Docker helps reduce these differences by packaging the application and its dependencies into an image.

**Main advantages:**
- Portability across compatible environments.
- Consistent application runtime.
- Faster and repeatable deployments.
- Efficient resource utilization.
- Isolation between applications.
- Easier integration with CI/CD pipelines.
- Simplified scaling and application management.

**Important:** Docker does not automatically solve every environment problem. Network configuration, environment variables, permissions, external services, and hardware architecture can still cause failures.

---

## 2. What Is the Difference Between Docker and a Virtual Machine?



This is one of the most frequently asked Docker interview questions.

| Feature | Docker Container | Virtual Machine |
|---|---|---|
| Architecture | Shares the host kernel in a typical Linux container setup | Runs a guest operating system with its own kernel |
| Size | Usually smaller | Usually larger |
| Startup | Generally fast | Generally slower |
| Resource usage | Often lower | Often higher |
| Isolation | Process-level isolation, depending on configuration | Stronger OS-level boundary in many configurations |
| Portability | Container images can be distributed between compatible environments | VM images can be moved between compatible virtualization platforms |
| Operating system | Linux containers require a compatible Linux kernel environment, directly or through a VM | Can run a different guest OS supported by the hypervisor |

### Interview-ready answer

The main difference between Docker containers and virtual machines is their architecture.

A virtual machine runs a complete guest operating system on top of a hypervisor. A typical Docker container shares the host operating system's kernel and isolates application processes.

Because containers don't require a separate guest operating system for every application, they generally use fewer resources and start faster than virtual machines.

Virtual machines provide a separate operating system environment and can offer a stronger isolation boundary in many configurations.

We choose between them based on application requirements, security, compatibility, and infrastructure needs.

**Remember:** Containers are not simply lightweight virtual machines. They use a different isolation and execution model.

---

## 3. Docker Architecture

Docker architecture consists of several important components.

| Component | Explanation |
|---|---|
| Docker Client | The interface through which users execute Docker commands |
| Docker Daemon | The background service that manages Docker objects |
| Docker Engine | The technology and components used to build and run containers |
| Docker Image | A read-only template used to create containers |
| Docker Container | A running or stopped instance created from an image |
| Docker Registry | A service that stores and distributes images |
| Dockerfile | A text file containing instructions for building an image |
| Docker Volume | A mechanism for persisting data outside a container's writable layer |
| Docker Network | A mechanism for connecting containers and other endpoints |

### How does Docker work?

Suppose you execute:

```bash
docker run -d -p 8080:8080 --name myapp nginx
```

Here's what happens:

1. The Docker client sends the request to the Docker daemon.
2. Docker checks whether the `nginx` image is available locally.
3. If it isn't available, Docker attempts to pull it from the configured registry.
4. Docker creates and starts a container.
5. Docker publishes host port `8080` to container port `8080`.
6. You can access the Nginx service through the host's port, provided networking and firewall rules permit it.

### Interview question: What is the difference between an image and a container?

**Answer:**

A Docker image is a template containing the application and its required filesystem layers and metadata. A container is an instance created from that image.

An image is generally immutable, while a container has a writable layer for runtime changes.

For example, we can build one Spring Boot image and use it to create multiple containers running the same application.

---

## 4. Important Docker Commands

You should know these commands for interviews and practical work.

### A. Check Docker installation and status

```bash
docker --version
docker info
docker ps
docker ps -a
```

| Command | Purpose |
|---|---|
| `docker --version` | Displays Docker CLI version |
| `docker info` | Displays Docker engine information |
| `docker ps` | Lists running containers |
| `docker ps -a` | Lists all containers, including stopped ones |

On a Linux system using systemd, you can check the service with:

```bash
sudo systemctl status docker
```

### B. Image commands

```bash
docker images
docker pull nginx
docker build -t myapp:latest .
docker tag myapp:latest username/myapp:v1
docker push username/myapp:v1
docker rmi myapp:latest
```

**Explanation:**

- `docker images` — lists local images.
- `docker pull` — downloads an image from a registry.
- `docker build` — builds an image from a Dockerfile.
- `docker tag` — assigns an additional name or tag to an image.
- `docker push` — uploads an image to a registry.
- `docker rmi` — removes an image if it is not in use in a way that prevents removal.

Before pushing to a registry, authenticate using the appropriate method and ensure you have permission to publish to the target repository.

### C. Container commands

```bash
docker run -d --name mycontainer nginx

docker ps
docker ps -a

docker stop mycontainer
docker start mycontainer
docker restart mycontainer

docker logs mycontainer
docker logs -f mycontainer

docker exec -it mycontainer sh

docker rm mycontainer
```

| Command | Purpose |
|---|---|
| `docker run` | Creates and starts a container |
| `docker stop` | Gracefully requests the container to stop |
| `docker start` | Starts an existing stopped container |
| `docker restart` | Restarts a container |
| `docker logs` | Displays container output |
| `docker exec` | Executes a command in a running container |
| `docker rm` | Removes a container |
| `docker inspect` | Displays detailed configuration and state |

**Interview question: What is the difference between `docker run` and `docker start`?**

**Answer:** `docker run` creates a new container from an image and starts it. `docker start` starts an existing stopped container.

### D. Port mapping

Example:

```bash
docker run -d \
  --name webapp \
  -p 8080:80 \
  nginx
```

Here:

- `8080` is the host port.
- `80` is the container port.
- Nginx listens on port 80 inside the container.
- Requests to host port 8080 are forwarded to container port 80.

The application can typically be accessed at `http://localhost:8080` on the Docker host, provided the service is running and the port is reachable.

**Important interview point:** `EXPOSE 80` in a Dockerfile documents the intended container port. It does not publish that port on the host. The `-p` option publishes it.

---

## 5. What Is a Dockerfile?

A Dockerfile is a text file containing instructions that Docker uses to build a container image. [Docker Documentation](https://docs.docker.com/reference/dockerfile?utm_source=chatgpt.com)

### Important Dockerfile instructions

| Instruction | Purpose |
|---|---|
| `FROM` | Selects the base image |
| `WORKDIR` | Sets the working directory |
| `COPY` | Copies files into the image |
| `ADD` | Copies files or retrieves supported remote resources |
| `RUN` | Executes commands during image building |
| `CMD` | Defines the default command or arguments when a container starts |
| `ENTRYPOINT` | Defines the main executable for the container |
| `EXPOSE` | Documents the port the application is expected to listen on |
| `ENV` | Sets environment variables in the image |
| `ARG` | Defines build-time variables |
| `USER` | Sets the user for subsequent build instructions and the default runtime user |
| `VOLUME` | Declares a mount point for persistent or externally managed storage |
| `HEALTHCHECK` | Defines a mechanism for checking container health |

### Example: Dockerfile for a Spring Boot application

Assume your Maven project generates this file:

```text
target/student-app.jar
```

Create a file named `Dockerfile` in the project root:

```dockerfile
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY target/student-app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

**Explanation:**

1. `FROM` selects a Java 17 runtime image.
2. `WORKDIR` sets `/app` as the working directory.
3. `COPY` copies the generated JAR into the image.
4. `EXPOSE` documents the application's intended listening port.
5. `ENTRYPOINT` starts the Spring Boot application.

This example assumes the application is configured to listen on port 8080. Adjust the Java version, JAR filename, and port to match your application.

### Build and run the application

Build the JAR first:

```bash
mvn clean package
```

Build the Docker image:

```bash
docker build -t student-app:latest .
```

Run the container:

```bash
docker run -d \
  --name student-container \
  -p 8080:8080 \
  student-app:latest
```

Check its status:

```bash
docker ps -a
```

Check its logs:

```bash
docker logs student-container
```

If the application is configured to listen on port 8080, you can test it at `http://localhost:8080` on the Docker host.

### Interview question: What is the difference between `RUN`, `CMD`, and `ENTRYPOINT`?

| Instruction | When it operates | Purpose |
|---|---|---|
| `RUN` | During image building | Executes build-time commands |
| `CMD` | When a container starts | Supplies the default command or arguments |
| `ENTRYPOINT` | When a container starts | Defines the main executable |

**Example:**

```dockerfile
FROM ubuntu:24.04

RUN apt-get update && apt-get install -y curl

CMD ["curl", "--version"]
```

Here, `RUN` installs curl while building the image. `CMD` specifies the default command when the container starts.

A common pattern is to combine `ENTRYPOINT` and `CMD`: the entrypoint defines the executable, while CMD supplies its default arguments.

---

## 6. What Is the Difference Between `COPY` and `ADD`?

Both instructions can place files into an image, but their capabilities differ.

- `COPY` copies files and directories from the build context into the image.
- `ADD` also supports additional behaviors, including extracting supported local archives and retrieving supported remote resources.

**Interview answer:** I generally prefer `COPY` when I only need to copy application files because its purpose is straightforward and explicit. I use `ADD` when its additional functionality is specifically needed.

## 7. What Are Docker Volumes?

By default, data stored only in a container's writable layer is not preserved after the container is removed.

A Docker volume provides storage managed by Docker that can persist beyond the lifecycle of an individual container. [Docker Documentation](https://docs.docker.com/learn/paths/get-started-with-docker-containers/?utm_source=chatgpt.com)

### Create and use a volume

```bash
docker volume create app-data

docker volume ls

docker run -d \
  --name db-container \
  -v app-data:/var/lib/postgresql/data \
  postgres:17
```

This is an illustrative example for a PostgreSQL container. In a real deployment, configure database credentials securely and use an appropriate PostgreSQL version and data directory.

### Types of storage

| Type | Explanation | Common use |
|---|---|---|
| Named volume | Storage managed by Docker | Database data |
| Bind mount | Maps a host path into a container | Development and configuration files |
| tmpfs mount | Stores data in host memory on supported Linux environments | Temporary, nonpersistent data |

**Interview question: Why do we use Docker volumes?**

Volumes allow data to persist independently of a container's writable layer. For example, a database container can be replaced without losing its data, provided the data is stored in the same correctly configured persistent volume.

---

## 8. What Is Docker Networking?

Docker networking allows containers to communicate with other containers, the host, and external systems.

### Types of Docker networks

| Network type | Purpose |
|---|---|
| Bridge | Connects containers on a Docker host; user-defined bridge networks support container-name DNS |
| Host | Uses the host's network namespace on supported Linux environments |
| None | Disables normal container networking |
| Overlay | Connects services across multiple Docker hosts when using a supported Swarm setup |
| Macvlan | Gives containers their own MAC addresses on a physical network |

### Important networking commands

```bash
docker network ls

docker network create app-network

docker network inspect app-network

docker network connect app-network mycontainer

docker network disconnect app-network mycontainer
```

### Practical example: Backend and database

Suppose you have two containers:

- Spring Boot backend
- PostgreSQL database

Create a network:

```bash
docker network create student-network
```

Run the database:

```bash
docker run -d \
  --name student-db \
  --network student-network \
  -e POSTGRES_DB=studentdb \
  -e POSTGRES_USER=appuser \
  -e POSTGRES_PASSWORD='replace-with-a-secure-secret' \
  postgres:17
```

Configure the Spring Boot application's database connection using:

```text
Host: student-db
Port: 5432
Database: studentdb
```

**Why use `student-db` instead of `localhost`?**

Inside the backend container, `localhost` refers to the backend container itself, not the PostgreSQL container.

On a shared user-defined bridge network, the backend can connect to the database using the database container's name.

For production, don't hardcode database passwords into Dockerfiles or commit them to GitHub. Use an appropriate secret-management mechanism.

### Interview-ready answer

Docker networking allows containers to communicate with each other and with external systems.

For example, in a Spring Boot application with a PostgreSQL database, we can create a user-defined bridge network and connect both containers to it.

The backend can communicate with the database using the database container's name and port.

We can also publish a container port to the host when external access is required.

Docker networking helps us separate application components and manage their communication.

---

## 9. What Is Docker Compose?

Docker Compose is a tool for defining and running applications consisting of multiple containers.

Instead of starting the backend, frontend, and database separately with lengthy commands, you can define their configuration in a `compose.yaml` file and manage them together.

### Example: Spring Boot + PostgreSQL

Create a file named `compose.yaml`:

```yaml
services:
  backend:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/studentdb
      SPRING_DATASOURCE_USERNAME: appuser
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:17
    environment:
      POSTGRES_DB: studentdb
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U appuser -d studentdb"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres-data:
```

This example assumes the backend Dockerfile is in the same directory and the Spring Boot application supports these environment variables.

Create a local `.env` file:

```dotenv
DB_PASSWORD=replace-with-a-secure-local-password
```

Add `.env` to `.gitignore` so that local credentials aren't committed to GitHub. For production, use an appropriate secrets-management solution rather than relying on a committed configuration file.

### Run the application

```bash
docker compose up -d --build
```

Check running services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs -f
```

Stop the services:

```bash
docker compose down
```

Stop the services and remove the Compose-managed volumes:

```bash
docker compose down -v
```

**Warning:** The last command can delete the database data stored in those Compose-managed volumes.

### Interview question: Docker vs. Docker Compose

**Answer:**

Docker provides the tools to build images and create and run containers. Docker Compose lets us define and manage a multi-container application through a YAML configuration file.

For example, we can use Docker commands to run an individual Spring Boot container, while Docker Compose can manage the backend and PostgreSQL database together.

**Reference:** [Docker Compose documentation](https://docs.docker.com/compose/?utm_source=chatgpt.com)

---

## 10. What Is a Docker Registry?

A Docker registry stores and distributes container images.

Examples include:

- Docker Hub
- Amazon Elastic Container Registry (Amazon ECR)
- Private container registries maintained by organizations.

### Image workflow

```text
Dockerfile
    |
    v
Build Image
    |
    v
Tag Image
    |
    v
Push to Registry
    |
    v
Pull Image on Deployment Server
    |
    v
Run Container
```

### Example commands

```bash
docker build -t student-app:v1 .

docker tag student-app:v1 username/student-app:v1

docker login

docker push username/student-app:v1
```

On the target server:

```bash
docker pull username/student-app:v1

docker run -d \
  --name student-app \
  -p 8080:8080 \
  username/student-app:v1
```

Replace `username` with the appropriate registry namespace and authenticate before pulling a private image.

**Interview question: Why do we push Docker images to a registry?**

We use a registry to store and distribute built images. This allows CI/CD systems to build an image once, publish it, and deploy the same version to different environments without rebuilding it on every server.

---

# 11. Docker in a Jenkins CI/CD Pipeline

This is especially important for your DevOps interview because you have practiced Jenkins pipelines and Docker deployments.

Consider this architecture:



### Complete workflow

1. A developer pushes code to GitHub.
2. Jenkins checks out the code.
3. Jenkins builds the Spring Boot application.
4. Jenkins runs automated tests.
5. Jenkins builds a Docker image.
6. Jenkins pushes the image to a container registry.
7. The deployment server pulls the required image version.
8. The application container starts.
9. Jenkins or a monitoring system verifies application health.

### Example Jenkinsfile

This is a simplified example for a Maven-based Spring Boot project that already contains a suitable Dockerfile.

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/USERNAME/REPOSITORY.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t student-app:latest .'
            }
        }

        stage('Verify Image') {
            steps {
                sh 'docker image inspect student-app:latest'
            }
        }
    }
}
```

This pipeline builds and verifies the image locally on the Jenkins agent. It does not yet push the image to a registry or deploy it to another EC2 instance.

For a real deployment pipeline, you would add registry authentication, image publishing, secure deployment credentials, remote deployment steps, and health verification.

**Important interview point:** Jenkins must have access to Docker, the build tools, and the required permissions. Avoid solving Docker permission issues by indiscriminately running all Jenkins jobs as root.

---

# 12. Docker Troubleshooting — Real Interview Scenarios

These scenarios are particularly useful because interviewers often want to know how you diagnose problems, not just how you define Docker.

## Scenario 1: Container exits immediately

**Question:** You run a Docker container, but it immediately exits. What do you do?

Start by checking the container status:

```bash
docker ps -a
```

Check the logs:

```bash
docker logs <container-name>
```

Inspect its exit status:

```bash
docker inspect <container-name>
```

Check the last exit code:

```bash
docker inspect -f '{{.State.ExitCode}}' <container-name>
```

### Common causes

- The application has crashed.
- The startup command is incorrect.
- Required environment variables are missing.
- The application cannot connect to a database.
- The container's main process has finished.
- The application is listening on a different port than expected.

**Interview answer:**

I first check the container status using `docker ps -a` and inspect the logs using `docker logs`. Then I examine the exit code, startup command, environment variables, and external dependencies. After identifying the root cause, I correct the configuration or application issue, rebuild the image if necessary, and test the container again.

## Scenario 2: Container is running, but the application is inaccessible

Check:

```bash
docker ps
docker port <container-name>
docker logs <container-name>
```

Then verify:

1. Is the application actually listening on the expected port?
2. Is the correct host-to-container port mapping configured?
3. Is the application listening on an address reachable from outside its container?
4. Is a host firewall blocking access?
5. If deployed on AWS EC2, does the security group permit the required inbound traffic?
6. Is the application healthy and responding to requests?

**Remember:** A running container does not necessarily mean its application is healthy.

## Scenario 3: Port is already allocated

You might see an error similar to:

```text
Bind for 0.0.0.0:8080 failed:
port is already allocated
```

Check the containers:

```bash
docker ps
```

You can use another host port:

```bash
docker run -d \
  --name student-app \
  -p 8081:8080 \
  student-app:latest
```

Now host port 8081 forwards to container port 8080.

If another process is using the host port, investigate it before changing or stopping anything.

## Scenario 4: Docker permission denied

Example error:

```text
permission denied while trying to connect to the Docker daemon socket
```

Check the Docker service:

```bash
sudo systemctl status docker
```

Check access to the Docker daemon:

```bash
docker info
```

If your account lacks permission, review the host's Docker access configuration.

**Security note:** Membership in the Docker group can grant root-equivalent privileges on a typical Linux host. Treat access to the Docker daemon as highly privileged. On a Jenkins server, grant access only to trusted jobs and users.

## Scenario 5: Docker image build fails

Useful commands:

```bash
docker build --no-cache -t student-app:debug .
```

```bash
docker images
```

Check:

- Whether the Dockerfile is present in the build context.
- Whether the base image can be downloaded.
- Whether the required source files exist.
- Whether dependency installation succeeds.
- Whether the application builds successfully before the Docker image is created.
- Whether `.dockerignore` excludes required files.

Use `--no-cache` when you need to determine whether cached build layers are contributing to the problem; it is not necessary for every failed build.

---

# 13. Docker Security and Best Practices

These concepts can help distinguish a basic answer from a strong interview answer.

### 1. Use trusted base images

Prefer well-maintained base images and keep them updated.

### 2. Use a `.dockerignore` file

Example:

```text
.git
.env
target/
node_modules/
*.log
```

Only exclude files that your build does not require. For example, if your Dockerfile copies a prebuilt JAR from `target/`, excluding `target/` would break the build unless you build the JAR inside the Dockerfile or use another build context.

### 3. Avoid running applications as root

Where possible, configure the container to run the application under a dedicated, unprivileged user.

### 4. Never hardcode credentials

Don't place passwords, API keys, or other secrets in Dockerfiles or source code.

### 5. Use small, efficient images

Multi-stage builds can separate build dependencies from the final runtime image, reducing unnecessary content.

### 6. Use specific image versions

Prefer an appropriate, maintained version or digest rather than relying blindly on a floating `latest` tag.

### 7. Store persistent data outside disposable containers

Use volumes or suitable external storage for important application data.

### 8. Monitor and inspect containers

Use logs, health checks, resource monitoring, and appropriate alerts to identify problems.

---

# 14. Top 20 Docker Interview Questions

Use this as a quick revision checklist.

| No. | Question | What your answer should cover |
|---:|---|---|
| 1 | What is Docker? | Containerization and portability |
| 2 | Why do we use Docker? | Consistency, deployment, isolation |
| 3 | What is a Docker image? | Read-only template |
| 4 | What is a Docker container? | Instance created from an image |
| 5 | Docker vs. VM? | Kernel sharing and architecture |
| 6 | What is a Dockerfile? | Instructions for building images |
| 7 | What is Docker Hub? | Image registry |
| 8 | What is a Docker daemon? | Manages Docker objects |
| 9 | `RUN` vs. `CMD`? | Build-time vs. runtime |
| 10 | `CMD` vs. `ENTRYPOINT`? | Default command/arguments vs. main executable |
| 11 | `COPY` vs. `ADD`? | Basic copying vs. additional capabilities |
| 12 | What is port mapping? | Host port to container port |
| 13 | What is a Docker volume? | Persistent storage |
| 14 | What is Docker networking? | Container communication |
| 15 | What is Docker Compose? | Multi-container applications |
| 16 | What is a multi-stage build? | Separate build and runtime stages |
| 17 | Why does a container exit? | Main process, logs, exit code |
| 18 | How do you troubleshoot a container? | Status, logs, inspect, configuration |
| 19 | How does Docker integrate with Jenkins? | Automated image build and deployment |
| 20 | How do you deploy Docker on AWS EC2? | Image transfer, container startup, security, health checks |

For official learning material, use the [Docker documentation](https://docs.docker.com/?utm_source=chatgpt.com) and its [Docker getting-started learning path](https://docs.docker.com/learn/paths/get-started-with-docker-containers/?utm_source=chatgpt.com).

---

# 15. Final Revision: Explain Docker in 60 Seconds

Practice this answer aloud until you can explain it naturally without memorizing every word.

Docker is a containerization platform that allows us to package applications and their dependencies into portable container images.

A Dockerfile contains the instructions used to build an image, and a container is an instance created from that image.

In DevOps, Docker is commonly integrated with Jenkins to automate application packaging and deployment. For example, Jenkins can build a Spring Boot application, create a Docker image, and publish it to a container registry. The image can then be deployed on an AWS EC2 instance.

Docker also provides networking for container communication and volumes for persistent data storage. Docker Compose helps us manage applications containing multiple containers.

Overall, Docker helps improve deployment consistency, portability, and automation while making application environments easier to manage.

----
# Kubernetes Notes for DevOps Interviews 🚀

These notes will help you understand **Kubernetes from the basics to practical DevOps scenarios**, especially how Kubernetes works with Docker, Jenkins, and AWS. They include important commands, real-world examples, and interview questions with answers.

Since you are already practising Jenkins and Docker pipelines, we’ll focus on how to deploy, manage, and troubleshoot applications in Kubernetes.

## 1. What is Kubernetes?

**Kubernetes (K8s)** is an open-source container orchestration platform used to deploy, manage, scale, and maintain containerized applications.

Imagine you have a Spring Boot application running inside a Docker container. You want to deploy it, run multiple replicas, restart it if it crashes, and distribute traffic between instances.

Kubernetes helps automate these tasks.

### Why do we need Kubernetes?

Suppose your application runs inside one Docker container.

If the container crashes:
- The application may become unavailable.
- You may need to restart the container manually.
- Handling multiple containers across multiple servers becomes difficult.

Kubernetes helps by providing:
- **Self-healing:** Restarts or replaces failed workloads.
- **Scaling:** Increases or decreases the number of application replicas.
- **Load balancing:** Distributes network traffic across application instances.
- **Rolling updates:** Updates applications gradually.
- **Service discovery:** Helps application components find one another.
- **Declarative management:** You describe the desired state, and Kubernetes works to maintain it.

**Interview answer:**

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications. It provides features such as self-healing, load balancing, service discovery, and rolling updates. In a DevOps environment, we can use Kubernetes with Docker images and Jenkins pipelines to automate application deployment and maintain application availability.

## 2. What is the difference between Docker and Kubernetes?

| Docker | Kubernetes |
|---|---|
| A platform and set of tools for building and running containers. | A platform for orchestrating containerized workloads. |
| Builds container images and runs containers. | Manages application workloads across a cluster. |
| Can run containers on a single machine. | Coordinates workloads across cluster nodes. |
| Provides commands such as `docker build` and `docker run`. | Provides commands such as `kubectl apply` and `kubectl get pods`. |
| Does not, by itself, provide Kubernetes-style cluster orchestration. | Supports scaling, self-healing, and rolling updates through its controllers. |

**Important:** Kubernetes does not require Docker Engine to run containers. Modern Kubernetes commonly uses **containerd** or another compatible container runtime through the Container Runtime Interface (CRI).

### How Docker, Jenkins, and Kubernetes work together

A typical DevOps workflow is:

1. A developer pushes code to GitHub.
2. Jenkins checks out the source code.
3. Jenkins builds and tests the application.
4. Jenkins builds a Docker image.
5. The image is pushed to a container registry, such as Docker Hub or Amazon ECR.
6. Jenkins updates the Kubernetes deployment.
7. Kubernetes schedules the application Pods and maintains the desired number of replicas.
8. Users access the application through a Service or Ingress, depending on the architecture.

Example architecture:

`Developer → GitHub → Jenkins → Docker Image → Container Registry → Kubernetes → Users`

---

## 3. Kubernetes Architecture

Kubernetes architecture consists of two main parts:

1. **Control Plane**
2. **Worker Nodes**



### A. Control Plane

The Control Plane manages the Kubernetes cluster and makes decisions about where applications should run.

| Component | Responsibility |
|---|---|
| **API Server** | Entry point for Kubernetes API requests. |
| **etcd** | Stores Kubernetes cluster state and configuration data. |
| **Scheduler** | Selects a suitable worker node for newly created Pods. |
| **Controller Manager** | Runs controllers that work to maintain the desired cluster state. |
| **Cloud Controller Manager** | Integrates Kubernetes with supported cloud-provider functionality, when configured. |

**Easy way to remember:**
- API Server = Communicates with the cluster.
- etcd = Stores cluster state.
- Scheduler = Selects a node.
- Controller Manager = Maintains the desired state.

### B. Worker Node

Worker nodes run your application workloads.

| Component | Responsibility |
|---|---|
| **Kubelet** | Ensures that Pods assigned to its node are running as specified. |
| **Container Runtime** | Runs containers, for example, containerd. |
| **Kube-proxy** | Implements Service networking rules on many cluster configurations. Some networking implementations replace its functionality. |
| **Pods** | The smallest deployable units in Kubernetes. |

**Interview question: What happens when you deploy an application to Kubernetes?**

1. You submit a manifest using `kubectl apply`.
2. The API Server validates and processes the request.
3. The desired state is stored in the cluster's data store.
4. Controllers observe the change and create or update the necessary resources.
5. The Scheduler selects a suitable node for each unscheduled Pod.
6. The Kubelet on the selected node works with the container runtime to start the containers.
7. Kubernetes monitors the workload and takes corrective action when necessary.

The exact sequence can vary depending on the resource and cluster state.

---

## 4. What Is a Kubernetes Cluster?

A **Kubernetes cluster** is a group of machines managed by Kubernetes.

It generally consists of a Control Plane and one or more worker nodes.

For example:

- Control Plane: Manages the cluster.
- Worker Node 1: Runs application Pods.
- Worker Node 2: Runs additional application Pods.

A cluster can run on local machines, virtual machines, or managed cloud services such as Amazon EKS.

### Important terms

- **Node:** A machine that participates in the cluster.
- **Pod:** The smallest deployable unit in Kubernetes.
- **Container:** The application process running inside a container environment.
- **Cluster:** The overall Kubernetes environment.

## 5. What Is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes. It contains one or more containers that share certain resources, including network identity and potentially mounted storage.

Most application Pods contain one main application container, although multi-container Pods are useful for tightly coupled components.

For example, you have a Spring Boot application packaged as a Docker image.

Kubernetes can run that image inside a Pod.

### Example Pod YAML

Create a file named `pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: springboot-pod
  labels:
    app: springboot
spec:
  containers:
    - name: springboot-container
      image: nginx:stable
      ports:
        - containerPort: 80
```

This example uses NGINX to demonstrate Pod creation. You would replace the image and port with the appropriate values for your Spring Boot application.

Apply the configuration:

```bash
kubectl apply -f pod.yaml
```

Check the Pod:

```bash
kubectl get pods
```

Get detailed information:

```bash
kubectl describe pod springboot-pod
```

View its logs:

```bash
kubectl logs springboot-pod
```

Delete the Pod:

```bash
kubectl delete pod springboot-pod
```

**Important:** In production, you would normally manage application Pods through a Deployment rather than creating standalone Pods.

---

## 6. What Is a Deployment?

A **Deployment** manages a set of replicated Pods and supports controlled application updates.

For example, suppose your application needs three running replicas.

You can configure a Deployment to maintain three Pods. If a managed Pod disappears, Kubernetes controllers work to restore the desired number of replicas.

### Example Deployment YAML

Create `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: springboot-deployment
spec:
  replicas: 3

  selector:
    matchLabels:
      app: springboot

  template:
    metadata:
      labels:
        app: springboot

    spec:
      containers:
        - name: springboot-container
          image: nginx:stable
          ports:
            - containerPort: 80
```

Apply the Deployment:

```bash
kubectl apply -f deployment.yaml
```

Check the Deployment:

```bash
kubectl get deployments
```

Check the Pods:

```bash
kubectl get pods
```

### Important Deployment commands

**Scale the application to five replicas:**

```bash
kubectl scale deployment springboot-deployment --replicas=5
```

**Check rollout status:**

```bash
kubectl rollout status deployment/springboot-deployment
```

**View rollout history:**

```bash
kubectl rollout history deployment/springboot-deployment
```

**Roll back to a previous revision:**

```bash
kubectl rollout undo deployment/springboot-deployment
```

**Delete the Deployment:**

```bash
kubectl delete deployment springboot-deployment
```

### Pod vs Deployment

| Pod | Deployment |
|---|---|
| Runs one or more containers. | Manages replicated Pods through ReplicaSets. |
| Does not independently provide replica management. | Maintains the configured number of replicas. |
| Can be created directly. | Creates and manages Pods based on its template. |
| Not normally used alone for production application management. | Commonly used for stateless application deployments. |

**Interview answer:**

A Kubernetes Deployment manages replicated Pods and helps maintain the desired number of application instances. It supports scaling, rolling updates, and rollbacks. For example, if I need to run three replicas of a Spring Boot application, I can define a Deployment with three replicas. Kubernetes then works to maintain the desired state of the application.

---

## 7. What Is a ReplicaSet?

A **ReplicaSet** ensures that a specified number of matching Pod replicas are running.

For example, if the desired replica count is three but only two matching Pods are available, the ReplicaSet controller attempts to create another Pod.

A Deployment normally manages ReplicaSets, so you generally create a Deployment rather than managing ReplicaSets directly.

**Remember the relationship:**

`Deployment → ReplicaSet → Pods → Containers`

---

## 8. What Is a Kubernetes Service?

A **Service** provides a stable network endpoint for accessing a group of Pods.

Pods can be recreated, and their IP addresses may change. A Service allows clients to access the application through a stable virtual IP or DNS name.

### Types of Services

| Service type | Purpose |
|---|---|
| **ClusterIP** | Exposes the Service within the cluster. This is the default type. |
| **NodePort** | Exposes the Service on a port on cluster nodes. |
| **LoadBalancer** | Requests an external load balancer when supported by the cluster environment. |
| **ExternalName** | Maps a Service name to an external DNS name. |

### Example Service YAML

Create `service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: springboot-service
spec:
  type: ClusterIP

  selector:
    app: springboot

  ports:
    - port: 80
      targetPort: 8080
```

Here:
- `port: 80` is the port exposed by the Service.
- `targetPort: 8080` is the destination port on the selected Pods.
- `selector` identifies the Pods that receive traffic.

**Note:** This is an illustrative example. The Service works only when matching Pods exist and the application listens on port `8080`.

Apply the Service:

```bash
kubectl apply -f service.yaml
```

Check Services:

```bash
kubectl get services
```

### Service vs Ingress

A Service provides stable access to application Pods.

An **Ingress** defines HTTP or HTTPS routing rules, such as directing requests for a hostname to a particular Service. It requires an Ingress controller to implement those rules.

For example:

- `app.example.com/api` → Backend Service.
- `app.example.com/` → Frontend Service.

Ingress is not itself a load balancer implementation; its behavior depends on the controller and infrastructure.

---

## 9. What Is a Namespace?

A **Namespace** logically separates resources within a Kubernetes cluster.

For example, a company might use:

- `dev` for development.
- `testing` for testing.
- `production` for production workloads.

Create a Namespace:

```bash
kubectl create namespace dev
```

List Namespaces:

```bash
kubectl get namespaces
```

Deploy a resource in a Namespace:

```bash
kubectl apply -f deployment.yaml -n dev
```

Check Pods in that Namespace:

```bash
kubectl get pods -n dev
```

Set a default Namespace for your current Kubernetes context:

```bash
kubectl config set-context --current --namespace=dev
```

**Interview question: Why do we use Namespaces?**

Namespaces help organize resources, apply access controls, and manage resource quotas within a cluster. They are useful for separating workloads and teams, but they are not a complete security boundary by themselves.

---

## 10. What Is a ConfigMap?

A **ConfigMap** stores non-confidential configuration data separately from application images.

Examples include:
- Application environment settings.
- Feature flags.
- Configuration files.
- Non-sensitive service endpoints.

Create a ConfigMap:

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=production
```

View it:

```bash
kubectl get configmap app-config -o yaml
```

You can consume ConfigMap values as environment variables or mounted files.

**Important:** Do not store passwords, API keys, or other confidential data in a ConfigMap.

## 11. What Is a Secret?

A **Secret** is a Kubernetes resource intended for sensitive data, such as credentials and tokens.

Example:

```bash
kubectl create secret generic db-secret \
  --from-literal=DB_USER=appuser \
  --from-literal=DB_PASSWORD='replace-with-a-secure-value'
```

Inspect its metadata:

```bash
kubectl describe secret db-secret
```

Kubernetes Secret values are commonly Base64-encoded when represented in YAML. **Base64 encoding is not encryption.**

For production, use appropriate access controls, encryption at rest, and a suitable secrets-management solution. In AWS, this might include AWS Secrets Manager with an appropriate integration.

**ConfigMap vs Secret**

| ConfigMap | Secret |
|---|---|
| Stores non-sensitive configuration. | Intended for sensitive configuration. |
| Values are not intended to be confidential. | Values require careful protection and access control. |
| Suitable for feature flags and application settings. | Suitable for credentials and tokens. |

---

## 12. What Is a PersistentVolume?

Containers and Pods can be replaced. Data that must survive Pod replacement needs suitable persistent storage.

Kubernetes provides several storage resources:

- **PersistentVolume (PV):** Represents storage available to the cluster.
- **PersistentVolumeClaim (PVC):** A request for storage by a workload.
- **StorageClass:** Defines a class of storage and can enable dynamic provisioning.

For example, an application might need persistent storage for database files.

In AWS, an EBS volume can be used with an appropriate Kubernetes storage driver and configuration. EBS volumes are generally attached to a single EC2 Availability Zone, which matters when scheduling Pods.

**Interview answer:**

A PersistentVolume represents storage available to a Kubernetes cluster, while a PersistentVolumeClaim is a request for storage by an application. A Pod can use a PVC to access persistent storage. This is important when application data must survive Pod recreation. Kubernetes can provision storage dynamically through a StorageClass when the storage system supports it.

---

## 13. What Is a StatefulSet?

A **StatefulSet** manages applications that need stable identities or persistent storage associations.

It is commonly used for workloads such as clustered databases and other stateful systems.

Unlike a typical Deployment, StatefulSet Pods have stable ordinal identities and can have individually associated persistent storage.

| Deployment | StatefulSet |
|---|---|
| Commonly used for stateless applications. | Used for applications needing stable identity or storage associations. |
| Pods can be replaced without stable individual identities. | Pods have stable, ordered identities. |
| Often appropriate for web servers and APIs. | Often appropriate for stateful clustered systems. |

A StatefulSet does not automatically make a database highly available. Database replication, backups, recovery, and consistency still require appropriate design.

---

## 14. What Is a DaemonSet?

A **DaemonSet** ensures that a Pod runs on each eligible node, or on a selected subset of nodes.

Common uses include:
- Log collection agents.
- Monitoring agents.
- Node-level networking components.
- Security agents.

For example, an organization might deploy a log-collection agent across all worker nodes.

**Interview question: What is the difference between a Deployment and a DaemonSet?**

A Deployment manages a desired number of Pod replicas. A DaemonSet manages Pods so that they run on every eligible node, or on nodes selected by its scheduling rules.

---

## 15. What Is a Job and CronJob?

### Job

A Job runs a task that is expected to complete successfully.

Examples:
- Database migration.
- Data processing.
- One-time maintenance task.

### CronJob

A CronJob creates Jobs on a recurring schedule.

Examples:
- Daily report generation.
- Scheduled cleanup.
- Periodic data processing.

Example CronJob:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: daily-task
spec:
  schedule: "0 2 * * *"

  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
            - name: task
              image: busybox:1.36
              command:
                - /bin/sh
                - -c
                - 'date; echo "Scheduled task executed"'
```

This example schedules a task for 2:00 AM according to the Kubernetes controller's time zone configuration, which is normally UTC unless configured otherwise.

---

## 16. Kubernetes Health Checks: Liveness, Readiness, and Startup Probes

These are important interview topics.

### Liveness Probe

Checks whether a container should be restarted because it is unhealthy.

### Readiness Probe

Checks whether a container is ready to receive traffic. A Pod that is not ready is normally excluded from the Service's ready endpoints.

### Startup Probe

Allows a slow-starting application time to initialize before liveness and readiness checks take effect.

| Probe | Main purpose |
|---|---|
| Liveness | Detect a container that needs restarting. |
| Readiness | Determine whether the application should receive traffic. |
| Startup | Allow the application sufficient time to start. |

**Practical example:** If a Spring Boot application takes 60 seconds to start, a suitable startup probe can prevent liveness checks from restarting it prematurely.

A failed readiness probe does not, by itself, restart the container.

---

## 17. Kubernetes Resource Requests and Limits

Kubernetes allows you to specify CPU and memory requirements for containers.

- **Requests:** The amount of CPU and memory used for scheduling decisions and resource allocation planning.
- **Limits:** The maximum CPU or memory resource usage permitted under the configured runtime enforcement.

Example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"

  limits:
    cpu: "500m"
    memory: "512Mi"
```

Here:
- `250m` CPU means one-quarter of a CPU core.
- `500m` CPU means half a CPU core.
- `256Mi` is 256 mebibytes of memory.
- `512Mi` is 512 mebibytes of memory.

If a container exceeds its memory limit, it may be terminated with an out-of-memory condition. CPU limit enforcement typically throttles CPU usage.

### What is HPA?

The **Horizontal Pod Autoscaler (HPA)** adjusts the number of replicas based on observed metrics and configured scaling targets.

For example, an HPA can increase the number of application Pods when CPU utilization rises.

Basic example:

```bash
kubectl autoscale deployment springboot-deployment \
  --cpu-percent=70 \
  --min=2 \
  --max=10
```

This command requests autoscaling between two and ten replicas with a target average CPU utilization of 70% relative to CPU requests. It requires suitable metrics to be available, and the Deployment must have appropriate CPU requests.

---

## 18. Essential Kubernetes Commands

Learn these commands for interviews and practical work.

### Cluster information

```bash
kubectl version
kubectl cluster-info
kubectl get nodes
kubectl describe node <node-name>
```

### Pods

```bash
kubectl get pods
kubectl get pods -o wide
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs -f <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
```

Use `/bin/bash` instead of `/bin/sh` only if Bash is installed in the container.

For a Pod containing multiple containers:

```bash
kubectl logs <pod-name> -c <container-name>
```

### Deployments

```bash
kubectl get deployments
kubectl describe deployment <deployment-name>
kubectl scale deployment <deployment-name> --replicas=3
kubectl rollout status deployment/<deployment-name>
kubectl rollout history deployment/<deployment-name>
kubectl rollout undo deployment/<deployment-name>
```

### Services

```bash
kubectl get services
kubectl get endpointslices
kubectl describe service <service-name>
```

EndpointSlices show the network endpoints associated with Services.

### Configuration

```bash
kubectl get configmaps
kubectl get secrets
kubectl get namespaces
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Apply and delete resources

```bash
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml
```

**Important:** Be careful with delete commands in production, particularly when they affect persistent storage or critical workloads.

---

## 19. Troubleshooting Kubernetes Pods

These scenarios are frequently discussed in DevOps interviews.

### Scenario 1: Pod is in `Pending` state

Possible causes:
- Insufficient CPU or memory on available nodes.
- PersistentVolumeClaim cannot be bound.
- Scheduling constraints cannot be satisfied.
- Node selectors, affinity rules, or taints prevent scheduling.

Commands:

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl get nodes
kubectl get events --sort-by=.metadata.creationTimestamp
```

Check the Events section of `describe pod` for scheduling errors.

### Scenario 2: Pod is in `CrashLoopBackOff`

This means a container is repeatedly failing and Kubernetes is backing off before restarting it again.

Possible causes:
- Application startup error.
- Incorrect environment variables.
- Database connection failure.
- Invalid configuration.
- Application process exits unexpectedly.

Commands:

```bash
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl describe pod <pod-name>
```

`--previous` is especially useful for retrieving logs from the previous terminated container instance.

### Scenario 3: Pod is in `ImagePullBackOff`

Possible causes:
- Incorrect image name or tag.
- Image does not exist.
- Private registry authentication is missing.
- Registry access or network problems.

Commands:

```bash
kubectl describe pod <pod-name>
kubectl get pods
```

Check the image name, tag, registry credentials, and Events output.

### Scenario 4: Pod is running, but the application is not accessible

Possible causes:
- Incorrect Service selector.
- Wrong `targetPort`.
- Application is listening on the wrong port.
- Application is bound only to localhost inside the container.
- Network policy or ingress configuration blocks traffic.

Commands:

```bash
kubectl get pods --show-labels
kubectl get services
kubectl describe service <service-name>
kubectl get endpointslices
kubectl logs <pod-name>
```

Confirm that the Service selects the correct Pods and that the application listens on the expected port.

### Scenario 5: Deployment update fails

Commands:

```bash
kubectl rollout status deployment/<deployment-name>
kubectl rollout history deployment/<deployment-name>
kubectl describe deployment <deployment-name>
kubectl get pods
```

If a previous revision is available and a rollback is appropriate:

```bash
kubectl rollout undo deployment/<deployment-name>
```

Investigate the underlying failure before attempting another rollout.

---

## 20. How to Deploy an Application Using Kubernetes

Let's consider a simple example involving a Spring Boot application.

### Step 1: Build a Docker image

```bash
docker build -t your-dockerhub-username/springboot-app:1.0 .
```

### Step 2: Push the image to a registry

```bash
docker login
docker push your-dockerhub-username/springboot-app:1.0
```

Use an appropriate registry authentication method and avoid storing credentials in source code.

### Step 3: Create a Deployment

Save the following as `springboot-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: springboot-app
spec:
  replicas: 2

  selector:
    matchLabels:
      app: springboot-app

  template:
    metadata:
      labels:
        app: springboot-app

    spec:
      containers:
        - name: springboot-app
          image: your-dockerhub-username/springboot-app:1.0
          ports:
            - containerPort: 8080
```

Replace the image name with your actual registry image. The application must listen on port `8080` for this example to work as intended.

Apply it:

```bash
kubectl apply -f springboot-deployment.yaml
```

### Step 4: Create a Service

Save the following as `springboot-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: springboot-service
spec:
  type: ClusterIP

  selector:
    app: springboot-app

  ports:
    - port: 8080
      targetPort: 8080
```

Apply it:

```bash
kubectl apply -f springboot-service.yaml
```

### Step 5: Verify the deployment

```bash
kubectl get deployments
kubectl get pods
kubectl get services
kubectl rollout status deployment/springboot-app
```

### Step 6: Test the application locally

For a basic test, use port forwarding:

```bash
kubectl port-forward service/springboot-service 8080:8080
```

Then send a request from another terminal:

```bash
curl http://localhost:8080
```

This request will work if the application has a suitable handler for the requested path.

---

## 21. How Jenkins Integrates with Kubernetes

Jenkins can automate application delivery to Kubernetes.

A typical pipeline contains these stages:

1. Checkout code from GitHub.
2. Build the application using Maven or Gradle.
3. Run automated tests.
4. Build a Docker image.
5. Push the image to a container registry.
6. Update the Kubernetes Deployment.
7. Verify rollout status.

### Example Jenkins Pipeline

This example assumes that Jenkins has:
- A configured Kubernetes context or access to a Kubernetes cluster.
- Permission to build container images.
- Registry credentials configured securely.
- `kubectl`, Docker, and Maven available in the execution environment as required.

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'your-dockerhub-username/springboot-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-username/your-repository.git'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PASSWORD" |
                            docker login -u "$DOCKER_USER" --password-stdin
                        docker push "$IMAGE_NAME:$IMAGE_TAG"
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl set image deployment/springboot-app \
                        springboot-app=$IMAGE_NAME:$IMAGE_TAG

                    kubectl rollout status deployment/springboot-app \
                        --timeout=180s
                '''
            }
        }
    }
}
```

**Before running this pipeline:**
- Replace the repository URL and Docker image name.
- Create the registry credential with the ID `dockerhub-credentials`.
- Ensure the Kubernetes Deployment already exists and its container is named `springboot-app`.
- Configure Kubernetes access securely for the Jenkins agent.
- Ensure the Jenkins identity has only the Kubernetes permissions it needs.

This pipeline updates an existing Deployment. It does not create the initial Deployment or Service, and it assumes the Docker image is available to the cluster.

For production, also configure cleanup and credential handling, restrict Kubernetes RBAC permissions, and consider using immutable image tags or image digests.

---

## 22. What Is Amazon EKS?

**Amazon Elastic Kubernetes Service (EKS)** is AWS's managed Kubernetes service.

AWS manages important parts of the Kubernetes Control Plane. You remain responsible for configuring and operating your workloads, worker compute, networking, access controls, and other resources according to your setup.

### EKS vs Kubernetes

| Kubernetes | Amazon EKS |
|---|---|
| Open-source container orchestration platform. | AWS-managed Kubernetes service. |
| Can be deployed in many environments. | Provides Kubernetes clusters on AWS. |
| Control Plane management depends on how it is deployed. | AWS manages the EKS Control Plane. |
| Infrastructure management depends on the environment. | Supports AWS integrations and multiple compute options. |

### Common EKS use cases

- Deploying Spring Boot microservices.
- Running containerized APIs.
- Scaling applications.
- Managing rolling updates.
- Integrating workloads with AWS networking and identity services.

You can learn more in the official [Amazon EKS documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html?utm_source=chatgpt.com).

---

## 23. Important Kubernetes Interview Questions

Practise answering these without looking at the notes.

| No. | Interview question | Key point to include |
|---:|---|---|
| 1 | What is Kubernetes? | Container orchestration. |
| 2 | Why do we use Kubernetes? | Scaling, self-healing, deployment management. |
| 3 | What is a Pod? | Smallest deployable unit. |
| 4 | What is a Deployment? | Manages replicated Pods and updates. |
| 5 | What is a ReplicaSet? | Maintains the desired number of matching Pods. |
| 6 | What is a Service? | Stable network access to Pods. |
| 7 | What is a Namespace? | Logical resource separation. |
| 8 | What is ConfigMap? | Non-sensitive configuration. |
| 9 | What is a Secret? | Resource intended for sensitive data. |
| 10 | What is a PV? | Represents persistent storage. |
| 11 | What is a PVC? | Requests storage. |
| 12 | What is a StatefulSet? | Stable identity and storage associations. |
| 13 | What is a DaemonSet? | Pod on each eligible node. |
| 14 | What is a Job? | Runs a task to completion. |
| 15 | What is a CronJob? | Schedules recurring Jobs. |
| 16 | What is HPA? | Scales replicas based on metrics. |
| 17 | What is a liveness probe? | Detects containers that need restarting. |
| 18 | What is a readiness probe? | Determines whether a Pod should receive traffic. |
| 19 | What is `CrashLoopBackOff`? | Repeated container failures and restart backoff. |
| 20 | What is `ImagePullBackOff`? | Kubernetes cannot pull the container image successfully. |
| 21 | What is `kubectl`? | Kubernetes command-line tool. |
| 22 | What is `etcd`? | Stores cluster state data. |
| 23 | What is the Scheduler? | Selects nodes for unscheduled Pods. |
| 24 | What is Ingress? | HTTP/HTTPS routing through an Ingress controller. |
| 25 | What is EKS? | AWS-managed Kubernetes service. |

---

## 24. Kubernetes Quick Revision Cheat Sheet

Remember these relationships:

- **Cluster** = The overall Kubernetes environment.
- **Control Plane** = Manages the cluster.
- **Worker Node** = Runs application workloads.
- **Pod** = Smallest deployable unit.
- **Deployment** = Manages replicated Pods and updates.
- **ReplicaSet** = Maintains the desired number of matching Pods.
- **Service** = Stable network endpoint for accessing Pods.
- **Ingress** = HTTP/HTTPS routing into Services through a controller.
- **Namespace** = Logical separation of resources.
- **ConfigMap** = Non-sensitive configuration.
- **Secret** = Sensitive configuration resource that needs protection.
- **PV/PVC** = Persistent storage and storage requests.
- **StatefulSet** = Workloads needing stable identity or storage associations.
- **DaemonSet** = Pod on each eligible node.
- **Job/CronJob** = One-time and scheduled tasks.
- **HPA** = Adjusts replica count using metrics.
- **EKS** = AWS-managed Kubernetes service.

Official reference: [Kubernetes Documentation](https://kubernetes.io/docs/home/?utm_source=chatgpt.com).

---

# Terraform Notes for DevOps Interviews
## Complete Revision Guide: Beginner to Practical Level

These notes will help you understand Terraform, revise important commands, explain concepts confidently in interviews, and troubleshoot real-world infrastructure problems.

We'll use AWS examples, particularly **EC2, IAM, Terraform state, and Jenkins CI/CD pipelines**, including the Terraform EC2 project you've practiced.

### Terraform revision roadmap

1. What is Terraform?
2. Infrastructure as Code (IaC)
3. Terraform architecture and workflow
4. Providers, resources, and data sources
5. Terraform configuration files
6. Variables, outputs, and locals
7. Terraform state
8. Terraform commands
9. AWS EC2 deployment project
10. Terraform modules
11. Remote state and state locking
12. Terraform with Jenkins
13. Common errors and troubleshooting
14. Top interview questions
15. Mock interview practice

---

# 1. What Is Terraform?

### Definition

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp. It allows us to define, provision, and manage infrastructure using configuration files.

Instead of manually creating AWS resources through the AWS Management Console, we can describe the required infrastructure in Terraform configuration files and apply those configurations.

### Example

Suppose you want to create an AWS EC2 instance.

**Without Terraform:**
1. Log in to AWS.
2. Open the EC2 service.
3. Select an Amazon Machine Image (AMI).
4. Choose an instance type.
5. Configure networking and security.
6. Launch the instance.

**With Terraform:**
1. Write an HCL configuration file.
2. Initialize Terraform.
3. Review the execution plan.
4. Apply the configuration.
5. Terraform creates the requested infrastructure.

### Interview-ready answer

Terraform is an Infrastructure as Code tool developed by HashiCorp. It allows us to provision and manage infrastructure using configuration files instead of creating resources manually.

Terraform supports multiple infrastructure providers, including AWS, Azure, and Google Cloud.

For example, we can use Terraform to create AWS EC2 instances, VPCs, security groups, and other infrastructure resources.

Its main advantages are automation, consistency, reusability, version control, and reduced manual errors. Terraform also provides an execution plan so we can review the proposed infrastructure changes before applying them.

**Keywords to remember:** Infrastructure as Code, HCL, providers, resources, state, plan, apply.

---

# 2. What Is Infrastructure as Code (IaC)?

Infrastructure as Code means defining and managing infrastructure through machine-readable configuration files rather than relying entirely on manual operations.

For example, you can define an AWS EC2 instance in a Terraform file and store that file in GitHub.

Your team can then review changes, track configuration history, and provision infrastructure using a repeatable workflow.

### Benefits of IaC

- **Automation:** Reduces repetitive manual tasks.
- **Consistency:** Helps create infrastructure according to the same configuration.
- **Version control:** Tracks infrastructure changes in Git.
- **Reusability:** Allows configurations to be reused through variables and modules.
- **Reviewability:** Terraform plans help teams inspect proposed changes.
- **Disaster recovery:** Infrastructure can be recreated from configuration, provided dependencies and required state or data are available.

### Interview question: Why do we use Terraform instead of creating infrastructure manually?

**Answer:**

Terraform automates infrastructure provisioning and makes configuration easier to version, review, and reuse. It also provides a plan before applying changes, helping us understand what Terraform intends to create, modify, or destroy.

Manual creation can work for small tasks, but it becomes harder to maintain consistency and track changes as infrastructure grows.

---

# 3. Terraform Architecture

Terraform's workflow involves several important components.

| Component | Explanation |
|---|---|
| Terraform CLI | Executes commands such as `init`, `plan`, and `apply` |
| Configuration files | Define the desired infrastructure |
| Provider | Connects Terraform to an infrastructure platform or service |
| Resource | Defines an infrastructure object Terraform manages |
| Data source | Reads information about an existing object or external system |
| State file | Records Terraform's representation of managed resources |
| Execution plan | Shows proposed changes before they are applied |
| Backend | Determines where Terraform state is stored and how it is managed |

### How does Terraform work?

Consider this example:

```text
Terraform Configuration
        |
        v
terraform init
        |
        v
terraform plan
        |
        v
Review Proposed Changes
        |
        v
terraform apply
        |
        v
AWS Provider API
        |
        v
AWS Infrastructure
```

Terraform uses providers to communicate with infrastructure platforms such as AWS. It compares configuration and state with information available from the provider to determine which changes are needed.

**Important:** Terraform does not simply execute every resource definition from scratch each time. It uses configuration, state, and refreshed information to determine the proposed changes.

---

# 4. Terraform Language: HCL

Terraform configuration is generally written in **HCL (HashiCorp Configuration Language)**.

A Terraform configuration commonly uses files with the `.tf` extension.

Important file types include:

| File | Purpose |
|---|---|
| `main.tf` | Defines resources and other main configuration |
| `provider.tf` | Defines provider configuration |
| `variables.tf` | Declares input variables |
| `outputs.tf` | Declares output values |
| `terraform.tfvars` | Supplies variable values |
| `.terraform.lock.hcl` | Records selected provider versions and checksums |
| `terraform.tfstate` | Stores Terraform state when using the local backend |

These are conventional filenames, not mandatory names for every configuration. Terraform loads `.tf` files from the working directory.

**Important:** State files can contain sensitive information. Never commit state files or credential-containing variable files to a public GitHub repository.

## 5. Terraform Providers

A provider is a plugin that enables Terraform to interact with a particular platform or service.

Examples include:

- AWS provider
- AzureRM provider
- Google Cloud provider
- Kubernetes provider

### Example: AWS provider

Create `main.tf`:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}
```

**Explanation:**

- `required_providers` declares which provider Terraform requires.
- `source` specifies the provider's registry address.
- `version` constrains acceptable provider versions.
- `provider "aws"` configures the AWS provider.
- `region` selects the AWS region.

The example uses a version constraint for AWS provider 5.x. For a new project, select an appropriate supported version, review compatibility, and commit the generated lock file.

### Interview question: What is a Terraform provider?

**Answer:**

A provider is a plugin that allows Terraform to communicate with an external platform or service through its API. For example, the AWS provider allows Terraform to create and manage resources such as EC2 instances, security groups, and S3 buckets.

---

# 6. Terraform Resources

A resource is a configuration block that defines an infrastructure object Terraform should manage.

Examples include:

- EC2 instances
- S3 buckets
- VPCs
- Security groups
- IAM roles

### Example: Create an AWS EC2 instance

```hcl
resource "aws_instance" "app_server" {
  ami           = "ami-xxxxxxxxxxxxxxxxx"
  instance_type = "t3.micro"

  tags = {
    Name = "Terraform-EC2-Instance"
  }
}
```

Replace the placeholder AMI with a valid, compatible AMI ID for your selected AWS region.

### Understanding the syntax

```hcl
resource "RESOURCE_TYPE" "RESOURCE_NAME" {
  # Configuration
}
```

For this example:

- `resource` declares a managed resource.
- `aws_instance` is the resource type.
- `app_server` is the Terraform-local resource name.
- `ami` selects the machine image.
- `instance_type` selects the EC2 instance type.
- `tags` assigns metadata to the instance.

Terraform refers to this resource as:

```text
aws_instance.app_server
```

### Interview question: What is the difference between a provider and a resource?

**Answer:**

A provider connects Terraform to a platform such as AWS, while a resource defines a specific object that Terraform manages on that platform.

For example, the AWS provider enables communication with AWS, and the `aws_instance` resource defines an EC2 instance.

---

# 7. Terraform Variables, Outputs, and Locals

These concepts make Terraform configurations more reusable and maintainable.

## A. Variables

Input variables allow you to pass values into a configuration without hardcoding every value into resource definitions.

Create `variables.tf`:

```hcl
variable "aws_region" {
  description = "AWS deployment region"
  type        = string
  default     = "ap-south-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t3.micro"
}
```

Use them in `main.tf`:

```hcl
provider "aws" {
  region = var.aws_region
}

resource "aws_instance" "app_server" {
  ami           = "ami-xxxxxxxxxxxxxxxxx"
  instance_type = var.instance_type
}
```

### Ways to supply variable values

**1. Default value**

```hcl
variable "instance_type" {
  type    = string
  default = "t3.micro"
}
```

**2. Using a `.tfvars` file**

```hcl
instance_type = "t3.small"
aws_region    = "ap-south-1"
```

Save these values in `terraform.tfvars`.

**3. Using the command line**

```bash
terraform plan -var="instance_type=t3.small"
```

**Interview question:** Why do we use variables in Terraform?

Variables help us reuse the same configuration with different values across environments, such as development, testing, and production.

## B. Outputs

Outputs display selected values after Terraform evaluates the configuration.

Create `outputs.tf`:

```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.app_server.id
}

output "public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.app_server.public_ip
}
```

After applying the configuration, you can inspect the outputs:

```bash
terraform output
terraform output instance_id
terraform output public_ip
```

An instance's public IP can be empty or unavailable if its configuration and networking do not provide one.

## C. Locals

Locals allow you to assign reusable values within a configuration.

```hcl
locals {
  project_name = "student-app"
  environment  = "dev"
}

resource "aws_instance" "app_server" {
  ami           = "ami-xxxxxxxxxxxxxxxxx"
  instance_type = "t3.micro"

  tags = {
    Name        = local.project_name
    Environment = local.environment
  }
}
```

### Variables vs. locals vs. outputs

| Feature | Purpose |
|---|---|
| Variables | Accept input values |
| Locals | Define reusable expressions inside the configuration |
| Outputs | Expose selected values from the configuration |

---

# 8. Terraform State File

**Terraform state is one of the most important interview topics.**

### What is Terraform state?

Terraform state records the relationship between resources declared in configuration and the real infrastructure Terraform manages.

By default, Terraform stores state locally in a file named `terraform.tfstate`, unless a different backend is configured.

For example, when Terraform creates an EC2 instance, the state records information that helps Terraform identify and manage that instance in subsequent operations.

### Why do we need Terraform state?

State helps Terraform:
- Associate configuration resources with real infrastructure.
- Track resource attributes and identifiers.
- Determine changes during planning.
- Understand dependencies between managed resources.
- Avoid treating every operation as a completely new deployment.

### Important state commands

```bash
terraform state list
```

Lists resources tracked in state.

```bash
terraform state show aws_instance.app_server
```

Displays information about a resource tracked in state.

```bash
terraform state pull
```

Retrieves the current state from the configured backend and writes it to standard output.

**Security warning:** State output may contain sensitive information. Don't paste it into public logs or repositories.

### Interview question: What happens if you delete the Terraform state file?

**Answer:**

If the state file is deleted, Terraform can lose its record of the resources it manages. The actual infrastructure usually remains in AWS, but Terraform may no longer know how the configured resources correspond to those existing objects.

If I run `terraform apply` without recovering or reconstructing the state, Terraform could propose creating duplicate resources or encounter conflicts.

I would first recover the state from a backup or remote backend. If recovery is not possible, I would carefully inspect the actual infrastructure and configuration and use Terraform import or other state-recovery procedures as appropriate.

**Important:** Never delete the state file as a routine troubleshooting step.

## 9. Local State vs. Remote State

| Local state | Remote state |
|---|---|
| Stored on the local filesystem | Stored in a configured remote backend |
| Convenient for individual practice | Better suited to many collaborative workflows |
| Requires you to manage backup and sharing | Backend can provide centralized access and supported locking |
| Can be lost with local disk failure | Depends on backend durability and backup configuration |

For an AWS team, an S3 backend is a common choice for centralized state storage. State locking must be configured according to the backend's supported mechanism and the Terraform version being used.

### Example S3 backend

```hcl
terraform {
  backend "s3" {
    bucket       = "my-terraform-state-bucket"
    key          = "dev/ec2/terraform.tfstate"
    region       = "ap-south-1"
    use_lockfile = true
  }
}
```

Before initializing this configuration, you must create the S3 bucket, configure appropriate permissions, and ensure the backend's locking and security requirements are satisfied.

This example uses S3-native state locking supported by current Terraform versions. Existing projects may use a different supported locking arrangement.

Initialize the backend:

```bash
terraform init
```

### What is state locking?

State locking helps prevent multiple Terraform operations from modifying the same state simultaneously when the configured backend supports locking.

Without appropriate coordination, concurrent changes can cause conflicts or inconsistent operations.

### Interview-ready answer

Terraform state is a record that maps resources in Terraform configuration to the real infrastructure being managed.

It helps Terraform identify existing resources and determine what changes are required during future plans and applies.

For small projects, state can be stored locally. For team environments, remote state storage, appropriate access controls, backups, and supported state locking help improve collaboration and reliability.

State files may contain sensitive information, so they must be protected and should not be committed to a public Git repository.

---

# 10. Terraform Commands You Must Remember

These are the essential commands for day-to-day Terraform work.

| Command | Purpose |
|---|---|
| `terraform -version` | Displays Terraform version |
| `terraform init` | Initializes the working directory and backend |
| `terraform fmt` | Formats configuration files |
| `terraform validate` | Checks configuration syntax and internal consistency |
| `terraform plan` | Previews proposed changes |
| `terraform apply` | Applies planned infrastructure changes |
| `terraform destroy` | Proposes and applies destruction of managed infrastructure |
| `terraform show` | Displays the current state or a saved plan |
| `terraform output` | Displays output values |
| `terraform state list` | Lists resources tracked in state |
| `terraform providers` | Displays provider requirements |
| `terraform refresh` | Deprecated; use the appropriate refresh-enabled plan or apply workflow |

### Recommended workflow

```bash
terraform init
terraform fmt -check
terraform validate
terraform plan
terraform apply
```

Review the plan before approving the apply operation.

To generate a saved plan:

```bash
terraform plan -out=tfplan
```

Apply that saved plan:

```bash
terraform apply tfplan
```

A saved plan can contain sensitive data. Protect it like state and don't commit it to GitHub.

### What does `terraform init` do?

It initializes the working directory, downloads the required providers, and configures the backend.

Run it when setting up a configuration for the first time or when required provider or backend configuration changes.

### What does `terraform plan` do?

It evaluates the proposed changes Terraform would make to bring the managed infrastructure in line with the configuration. It helps you review planned creations, updates, and deletions before applying them.

### What does `terraform apply` do?

It executes the approved plan, or creates and applies a plan when one isn't supplied, to make the infrastructure changes.

### What does `terraform destroy` do?

It plans and performs the destruction of infrastructure managed by the configuration and state, subject to dependencies and the selected operation.

**Interview tip:** Never describe `terraform destroy` as simply stopping an EC2 instance. It is intended to destroy managed infrastructure resources.

---

# 11. Practical Project: Create an AWS EC2 Instance Using Terraform

Let's revise a project similar to your previous Terraform EC2 pipeline.

### Project objective

Create an EC2 instance in AWS using Terraform and manage its lifecycle through Terraform commands.

### Prerequisites

- An AWS account.
- Terraform installed.
- AWS credentials configured securely.
- An AMI valid for the selected region.
- Appropriate IAM permissions.
- An AWS region and instance type supported by your account.

## Step 1: Create the project directory

```bash
mkdir terraform-ec2-project
cd terraform-ec2-project
```

Create a file named `main.tf`.

## Step 2: Write the Terraform configuration

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "app_server" {
  ami           = "ami-xxxxxxxxxxxxxxxxx"
  instance_type = "t3.micro"

  tags = {
    Name        = "Jenkins-Terraform-Instance"
    Environment = "Dev"
  }
}

output "instance_id" {
  value = aws_instance.app_server.id
}

output "public_ip" {
  value = aws_instance.app_server.public_ip
}
```

Replace the placeholder AMI ID with a valid AMI for `ap-south-1`.

**Note:** This is a minimal infrastructure example. It does not configure an application, an SSH key, or an application-specific security group.

## Step 3: Configure AWS credentials

For local practice, one option is to configure the AWS CLI:

```bash
aws configure
```

Enter the requested AWS access key, secret key, region, and output format.

Use credentials from an authorized IAM identity with the required permissions. Prefer temporary credentials or an appropriate IAM role when available.

**Never commit AWS credentials to GitHub or put them directly into Terraform configuration files.**

Verify the identity:

```bash
aws sts get-caller-identity
```

If you're running Terraform on an EC2 instance with an attached IAM role, the AWS provider can use the instance's credentials without requiring long-lived access keys in the configuration.

## Step 4: Initialize Terraform

```bash
terraform init
```

Terraform downloads the required AWS provider and initializes the working directory.

## Step 5: Validate the configuration

```bash
terraform fmt
terraform validate
```

- `fmt` formats the Terraform files.
- `validate` checks the configuration for errors.

## Step 6: Review the plan

```bash
terraform plan
```

Review the proposed EC2 creation and verify the AMI, region, instance type, and expected cost.

## Step 7: Create the instance

```bash
terraform apply
```

Review the proposed changes and confirm the operation when prompted.

## Step 8: Check the outputs

```bash
terraform output
```

You should see the instance ID and, if available, its public IP.

You can also verify the instance through the AWS EC2 console.

## Step 9: Destroy the infrastructure when finished

```bash
terraform destroy
```

Review the destruction plan before confirming.

**Cost warning:** EC2 instances and associated resources may incur charges. Destroy resources you no longer need, and verify that any other billable resources created during the exercise have also been handled.

### How to explain this project in an interview

I worked on a project to provision AWS EC2 infrastructure using Terraform.

First, I configured the AWS provider and defined an EC2 instance as a Terraform resource. I then initialized the working directory, validated the configuration, and used terraform plan to review the proposed infrastructure changes.

After reviewing the plan, I used terraform apply to create the instance and checked the output values to verify the deployment.

I also practiced managing the infrastructure lifecycle using Terraform state and terraform destroy.

This project helped me understand Infrastructure as Code, provider configuration, resource management, state handling, and automated infrastructure provisioning.

Only describe steps you've actually completed as hands-on experience. If you haven't performed a step yourself yet, describe it as something you learned or plan to implement.

---

# 12. Terraform with Jenkins CI/CD

This is an important topic for DevOps interviews because you have already practiced Jenkins and Terraform together.

### Architecture

```text
Developer
    |
    v
GitHub Repository
    |
    v
Jenkins Pipeline
    |
    v
Checkout Terraform Code
    |
    v
Terraform Init
    |
    v
Terraform Validate
    |
    v
Terraform Plan
    |
    v
Approval (if required)
    |
    v
Terraform Apply
    |
    v
AWS Infrastructure
```

### Example Jenkins pipeline

The following example assumes that Terraform is installed on the Jenkins agent and that the agent has an authorized AWS identity.

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/USERNAME/REPOSITORY.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'terraform fmt -check -recursive'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
    }
}
```

This example validates and plans infrastructure changes. It does not automatically apply them.

### Why separate plan and apply?

Separating planning from application makes it easier to review potentially destructive changes before they affect infrastructure.

For a production pipeline, use an appropriate approval process, protect credentials, restrict permissions, and ensure that the plan being approved is the plan actually applied.

### Important Jenkins troubleshooting scenario

Suppose the Jenkins pipeline fails with:

```text
No valid credential sources found
```

Possible causes include:

- AWS credentials are not configured for the Jenkins execution identity.
- The expected environment variables are missing.
- The Jenkins agent does not have an appropriate instance role.
- The credentials provider chain cannot access the expected identity.
- The identity lacks the required AWS permissions.

Useful commands for diagnosis include:

```bash
aws sts get-caller-identity
```

And, from the same execution environment:

```bash
terraform plan
```

The first command verifies the AWS identity visible to the AWS CLI; Terraform's provider configuration and execution environment should also be checked.

**Important:** A successful Jenkins job does not necessarily mean Terraform infrastructure was created successfully. Check which stages ran and whether an apply operation actually completed.

---

# 13. Terraform Modules

### What is a Terraform module?

A module is a collection of Terraform configuration files organized to manage related infrastructure.

The root module is the configuration in your current working directory. You can also create child modules and reuse them from other configurations.

For example, you might create separate modules for:

- VPC and networking
- EC2 instances
- Security groups
- RDS databases
- S3 buckets

### Example directory structure

```text
terraform-project/
├── main.tf
├── variables.tf
├── outputs.tf
└── modules/
    └── ec2/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

### Why use modules?

Modules improve reusability, organization, consistency, and maintainability. Instead of repeating the same EC2 configuration across multiple environments, you can create a reusable module with configurable inputs.

### Interview-ready answer

Terraform modules are collections of configuration files that manage related infrastructure resources.

The main configuration is called the root module, and reusable modules can be called from it.

For example, I can create an EC2 module that accepts an AMI ID and instance type as input variables and returns the instance ID as an output.

Modules help reduce code duplication, improve reusability, and make infrastructure easier to maintain across different environments.

---

# 14. Terraform Resource Dependencies

Terraform must determine the order in which resources should be created, updated, or destroyed.

There are two important types of dependencies.

### 1. Implicit dependency

Terraform automatically identifies a dependency when one resource references another resource's attribute.

Example:

```hcl
resource "aws_security_group" "app_sg" {
  name = "student-app-sg"
}

resource "aws_instance" "app_server" {
  ami                    = "ami-xxxxxxxxxxxxxxxxx"
  instance_type          = "t3.micro"
  vpc_security_group_ids = [aws_security_group.app_sg.id]
}
```

The EC2 instance references the security group's ID, so Terraform knows that the security group must exist before it can create the instance with that group.

This simplified example omits ingress and egress rules and assumes the security group is suitable for the instance.

### 2. Explicit dependency

Sometimes a dependency is required even when there is no direct resource-attribute reference.

In that case, you can use `depends_on`.

```hcl
resource "aws_instance" "app_server" {
  ami           = "ami-xxxxxxxxxxxxxxxxx"
  instance_type = "t3.micro"

  depends_on = [
    aws_security_group.app_sg
  ]
}
```

Use explicit dependencies only when Terraform cannot infer the required relationship automatically.

### Interview question: What is `depends_on`?

**Answer:** `depends_on` explicitly tells Terraform that a resource or module depends on another object. It helps control operation ordering when that dependency cannot be inferred from references in the configuration.

---

# 15. Terraform Lifecycle Rules

Terraform lifecycle rules control certain aspects of how Terraform handles resource changes.

Important lifecycle arguments include:

| Argument | Purpose |
|---|---|
| `create_before_destroy` | Requests that a replacement resource be created before the old one is destroyed, where possible |
| `prevent_destroy` | Rejects plans that would destroy the resource while this rule remains configured |
| `ignore_changes` | Tells Terraform to ignore changes to specified attributes when considering updates |

### Example: Prevent accidental destruction

```hcl
resource "aws_instance" "app_server" {
  ami           = "ami-xxxxxxxxxxxxxxxxx"
  instance_type = "t3.micro"

  lifecycle {
    prevent_destroy = true
  }
}
```

This can help protect a resource against accidental destruction through Terraform while the rule is present.

However, it is not a complete protection mechanism against every possible deletion method.

### Interview question: What is `create_before_destroy`?

**Answer:** It is a lifecycle rule that requests Terraform to create a replacement resource before destroying the existing one. It can help reduce downtime during certain replacements, provided the infrastructure and naming constraints allow both resources to coexist.

---

# 16. Terraform vs. Ansible vs. CloudFormation

Interviewers sometimes ask you to compare infrastructure tools.

| Feature | Terraform | Ansible | AWS CloudFormation |
|---|---|---|---|
| Primary purpose | Provision and manage infrastructure | Configuration management and automation | Provision and manage AWS infrastructure |
| Configuration | HCL | YAML and other supported formats | YAML or JSON |
| Typical use | Create EC2, VPC, databases, and other infrastructure | Configure servers, install packages, deploy applications | Define AWS resource stacks |
| State approach | Uses state to track managed resources | Does not use Terraform-style resource state | AWS manages stack state |
| Multi-cloud support | Supports multiple providers | Can automate many systems and platforms | Primarily AWS infrastructure |

These tools can complement each other. For example, Terraform can create an EC2 instance, while Ansible configures software on it.

### Interview answer

Terraform is primarily used for infrastructure provisioning and management. Ansible is commonly used for configuration management and operational automation. AWS CloudFormation is AWS's native infrastructure-as-code service.

The right choice depends on the infrastructure platform, automation requirements, team expertise, and existing workflows.

---

# 17. Common Terraform Errors and Solutions

These are particularly useful for practical interviews.

## Error 1: No valid credential sources found

**Possible causes:**
- Missing AWS credentials.
- Incorrect execution identity.
- Missing or inaccessible IAM role.
- Incorrect environment configuration.

**Troubleshooting:**

```bash
aws sts get-caller-identity
```

Then check the credentials available to the Terraform execution environment and confirm the required IAM permissions.

## Error 2: Error acquiring the state lock

**Possible causes:**
- Another Terraform operation is running.
- A previous operation failed while holding a lock.
- The backend's locking mechanism is unavailable or misconfigured.

**Solution:**

1. Check whether another Terraform process is active.
2. Inspect the backend and locking error.
3. Wait for a legitimate operation to finish.
4. If a stale lock remains, verify that it is safe to remove it.

Do not force-unlock a state while another operation is active. Forced unlocking is appropriate only when you have verified that the lock is stale and that no competing operation is modifying the state.

## Error 3: Error acquiring state lock because of permissions

Check the IAM permissions and backend configuration. For an S3 backend, confirm access to the relevant state object and, when applicable, its lock file.

## Error 4: Invalid AMI ID

**Possible causes:**
- The AMI doesn't exist in the selected region.
- The AMI ID was copied incorrectly.
- The AMI is not accessible to your account.
- The AMI is incompatible with the requested configuration.

**Solution:** Verify the AMI ID and region, and confirm that the AMI supports the selected architecture and instance configuration.

## Error 5: Resource already exists

Terraform may attempt to create a resource that already exists but isn't tracked in the relevant state.

**Solution:** Investigate the existing resource and determine whether it should be managed by this configuration. If appropriate, import it into Terraform state using the supported import workflow rather than deleting or recreating it blindly.

## Error 6: Terraform state permission denied

Example:

```text
Error: Failed to save state
Permission denied
```

**Possible causes:**
- The current user cannot write to the state file.
- Jenkins and a shell session use different operating-system users.
- Workspace files have incorrect ownership or permissions.

**Solution:** Check the execution identity and ownership of the workspace files.

```bash
whoami
pwd
ls -l
```

For Jenkins, run Terraform under the intended Jenkins agent identity and ensure that the workspace and backend are configured with appropriate permissions.

Avoid using `chmod 777` as a general fix. Correct ownership and least-privilege permissions are safer.

---

# 18. Top 25 Terraform Interview Questions

Use these as a revision checklist.

| No. | Interview question | Key point |
|---:|---|---|
| 1 | What is Terraform? | Infrastructure as Code |
| 2 | What is IaC? | Infrastructure defined through code |
| 3 | What is HCL? | Terraform configuration language |
| 4 | What is a provider? | Connects Terraform to a platform |
| 5 | What is a resource? | Managed infrastructure object |
| 6 | What is a data source? | Reads existing or external information |
| 7 | What does `terraform init` do? | Initializes the working directory |
| 8 | What does `terraform plan` do? | Previews changes |
| 9 | What does `terraform apply` do? | Applies infrastructure changes |
| 10 | What does `terraform destroy` do? | Destroys managed infrastructure |
| 11 | What is Terraform state? | Tracks resource relationships |
| 12 | What is remote state? | State stored in a remote backend |
| 13 | What is state locking? | Coordinates state-changing operations |
| 14 | What is a module? | Reusable configuration collection |
| 15 | What are variables? | Inputs to configuration |
| 16 | What are outputs? | Expose selected configuration values |
| 17 | What are locals? | Internal reusable expressions |
| 18 | What is `.terraform.lock.hcl`? | Records provider selections and checksums |
| 19 | What is a backend? | Configures state storage and related behavior |
| 20 | What is `depends_on`? | Declares explicit dependencies |
| 21 | What is `prevent_destroy`? | Guards against configured Terraform destruction |
| 22 | What is drift? | Difference between actual infrastructure and declared configuration |
| 23 | What is Terraform import? | Brings existing infrastructure under Terraform management |
| 24 | How does Terraform work with Jenkins? | Automates infrastructure workflows |
| 25 | How do you troubleshoot AWS authentication errors? | Check identity, credentials, roles, and permissions |

---

# 19. Quick Revision: Explain Terraform in 60 Seconds

Practice this answer aloud.

Terraform is an Infrastructure as Code tool developed by HashiCorp that allows us to provision and manage infrastructure using configuration files written in HCL.

It uses providers to communicate with platforms such as AWS and resources to define infrastructure components like EC2 instances, security groups, and S3 buckets.

The typical workflow involves terraform init, terraform plan, and terraform apply. Initialization prepares the working directory, planning previews the proposed changes, and applying executes the approved infrastructure changes.

Terraform state tracks the resources being managed, while variables and modules help us build reusable configurations.

In DevOps, we can integrate Terraform with Jenkins to automate infrastructure provisioning and manage infrastructure changes consistently through a CI/CD workflow.

---
# Jenkins Notes for DevOps Interviews 🚀

These notes cover Jenkins from basic concepts to practical CI/CD pipelines, Docker integration, Kubernetes deployment, and troubleshooting. They are designed to help you understand Jenkins and explain it confidently in DevOps interviews.

## 1. What is Jenkins?

Jenkins is an open-source automation server used to automate software development tasks, especially Continuous Integration (CI) and Continuous Delivery or Deployment (CD).

For example, when a developer pushes Java Spring Boot code to GitHub, Jenkins can automatically:

1. Download the source code from GitHub.
2. Build the application using Maven.
3. Run automated tests.
4. Build a Docker image.
5. Push the image to Docker Hub or Amazon ECR.
6. Deploy the application to an EC2 server or Kubernetes cluster.

### Interview-ready answer

Jenkins is an open-source automation server used to implement CI/CD pipelines. It automates tasks such as source-code checkout, application building, testing, packaging, and deployment. In my DevOps projects, I use Jenkins with GitHub, Maven, Docker, and cloud infrastructure to automate application delivery, reduce manual effort, and improve deployment consistency.

## 2. What is CI/CD?

Continuous Integration (CI)

Developers frequently merge code changes into a shared repository. Automated builds and tests help detect problems early.

Continuous Delivery

Code is automatically built, tested, and prepared for release. Production deployment may still require manual approval.

Continuous Deployment

Every change that passes the required checks can be deployed automatically to production, without a manual release approval.

### Typical CI/CD workflow

`Developer → GitHub → Jenkins → Build → Test → Docker Image → Registry → Deployment`

Interview tip: CI is about integrating and validating changes. Continuous Delivery prepares releases; Continuous Deployment automatically releases qualifying changes to production.

## 3. Jenkins Architecture

Jenkins commonly uses a controller-agent architecture.

[notes.kodekloud.com](https://notes.kodekloud.com/docs/Jenkins-For-Beginners/Jenkins-Setup-and-Interface/Jenkins-Architecture/page?utm_source=chatgpt.com)

| Component          | Responsibility                                                                    |
| ------------------ | --------------------------------------------------------------------------------- |
| Jenkins Controller | Provides the UI, schedules jobs, stores configuration, and coordinates execution. |
| Jenkins Agent      | Executes build and deployment tasks assigned by the controller.                   |
| Executor           | A slot on a controller or agent that can run a task.                              |
| Workspace          | Directory where a job checks out code and works with build files.                 |
| Plugin             | Extends Jenkins with integrations and additional capabilities.                    |

### Controller vs Agent

- Controller: Manages Jenkins and schedules work.
- Agent: Runs the actual build or deployment commands.
- Workspace: Holds the files used during a particular job execution.

For example, Jenkins can run on one EC2 instance while a separate agent builds Docker images or deploys applications.

Important: For production, avoid running untrusted builds on the controller. Use appropriately isolated agents and restrict their permissions.

## 4. What is a Jenkins Job?

A Jenkins Job is a configured unit of work that Jenkins executes.

Common types include:

- Freestyle Project: Configured through the Jenkins UI.
- Pipeline: Defines a workflow as code.
- Multibranch Pipeline: Discovers branches containing a `Jenkinsfile` and can run separate pipelines for them.
- Organization Folder: Discovers repositories and their branches or pipelines within a supported source-control organization.

For modern CI/CD projects, Pipeline and Multibranch Pipeline are especially useful because workflows can be stored and version-controlled alongside application code.

## 5. What is a Jenkinsfile?

A `Jenkinsfile` is a text file that defines a Jenkins Pipeline, usually written in Groovy-based Declarative or Scripted Pipeline syntax.

It is commonly stored in the root of a Git repository.

Example:

```
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-user/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```

This pipeline checks out code, packages the application, and runs tests. The build stage may already run tests depending on the Maven lifecycle and project configuration, so a separate test stage is shown mainly for clarity.

Replace the repository URL and ensure Maven and Java are installed or available on the selected agent.

### Advantages of a Jenkinsfile

- Pipeline is stored as code.
- Changes can be reviewed through Git.
- Pipelines are reproducible and easier to maintain.
- Supports multiple stages and conditional workflows.
- Makes it easier to standardize CI/CD across projects.

## 6. Declarative Pipeline vs Scripted Pipeline

| Declarative                                                    | Scripted                                                      |
| -------------------------------------------------------------- | ------------------------------------------------------------- |
| Uses a structured `pipeline {}` syntax.                        | Uses a more flexible `node {}` and Groovy scripting approach. |
| Easier to read and maintain for common pipelines.              | Offers greater programmatic flexibility.                      |
| Has built-in structures such as `stages`, `steps`, and `post`. | Often requires more explicit flow and error handling.         |
| A good default for most new pipelines.                         | Useful for advanced or highly dynamic logic.                  |

### Basic Declarative Pipeline structure

```
pipeline {
    agent any

    environment {
        APP_NAME = 'student-app'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building ${APP_NAME}"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
```

## 7. Important Jenkins Pipeline Keywords

| Keyword           | Meaning                                                   |
| ----------------- | --------------------------------------------------------- |
| `pipeline`        | Defines a Declarative Pipeline.                           |
| `agent`           | Specifies where the pipeline or stage runs.               |
| `stages`          | Groups the pipeline stages.                               |
| `stage`           | Defines a logical step in the workflow.                   |
| `steps`           | Contains the commands or actions for a stage.             |
| `sh`              | Executes shell commands on Unix-like agents.              |
| `bat`             | Executes Windows batch commands.                          |
| `environment`     | Defines environment variables.                            |
| `parameters`      | Defines user-supplied build parameters.                   |
| `tools`           | Selects configured tools where supported.                 |
| `when`            | Controls whether a stage should run.                      |
| `post`            | Defines actions after the pipeline or stage completes.    |
| `input`           | Requests human input or approval.                         |
| `withCredentials` | Makes configured credentials available to a scoped block. |
| `parallel`        | Runs independent branches of work concurrently.           |

### Common `post` conditions

- `always`: Run regardless of the outcome.
- `success`: Run after a successful result.
- `failure`: Run after a failure.
- `unstable`: Run when the result is unstable.
- `changed`: Run when the result differs from the previous build.

## 8. Jenkins Installation and Basic Commands

Jenkins can be installed on Linux, Windows, a container, or a Kubernetes environment. On an Ubuntu-based EC2 instance, the exact installation steps depend on the Jenkins release and Java requirements.

Useful Linux commands:

```
sudo systemctl status jenkins
sudo systemctl start jenkins
sudo systemctl stop jenkins
sudo systemctl restart jenkins
sudo systemctl enable jenkins
```

Check the service logs:

```
sudo journalctl -u jenkins -f
```

On common Linux installations, Jenkins listens on port `8080` by default, unless configured differently.

Security tip: Do not expose Jenkins publicly without appropriate network restrictions, HTTPS, authentication, and access controls.

## 9. Jenkins Plugins

Plugins add integrations and features to Jenkins.

| Plugin                 | Purpose                                                   |
| ---------------------- | --------------------------------------------------------- |
| Git                    | Integrates Git source control.                            |
| Pipeline               | Supports Pipeline jobs and syntax.                        |
| Credentials Binding    | Exposes credentials safely within build steps.            |
| Docker-related plugins | Add specific Docker integrations.                         |
| SSH-related plugins    | Support certain SSH-based workflows.                      |
| JUnit                  | Publishes JUnit-format test reports.                      |
| Blue Ocean             | Provides an alternative Pipeline visualization interface. |
| Kubernetes             | Supports Kubernetes-based agent integration.              |

Install only the plugins you need, keep them updated, and review their permissions and compatibility.

## 10. Jenkins Credentials Management

Jenkins Credentials stores secrets such as passwords, tokens, SSH keys, and cloud credentials.

Examples include:

- GitHub access tokens.
- Docker Hub credentials.
- SSH private keys.
- AWS access credentials.

Credentials can be configured under Manage Jenkins → Credentials, though the exact navigation can vary by Jenkins version and installed plugins.

Example:

```
withCredentials([
    usernamePassword(
        credentialsId: 'dockerhub-credentials',
        usernameVariable: 'DOCKER_USER',
        passwordVariable: 'DOCKER_PASSWORD'
    )
]) {
    sh '''
        set +x
        echo "$DOCKER_PASSWORD" |
            docker login -u "$DOCKER_USER" --password-stdin
        docker push your-user/student-app:latest
        docker logout
    '''
}
```

The credential ID must exist, and the credential type must match the binding. Replace the image name with your own.

Best practices:

- Never commit passwords or tokens to GitHub.
- Avoid printing secrets in console output.
- Use short-lived credentials or workload identities where supported.
- Apply least-privilege access.
- Restrict who can configure jobs that use sensitive credentials.

## 11. Integrating Jenkins with GitHub

There are two common approaches:

Webhook-based triggering: GitHub sends a webhook to Jenkins when a relevant event occurs. Jenkins must be reachable from the webhook sender, and the webhook endpoint must be configured securely.

Polling: Jenkins checks the repository periodically for changes.

Example polling configuration in a Declarative Pipeline:

```
pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Checking source changes and building.'
            }
        }
    }
}
```

This checks for changes approximately every five minutes. It is not the same as a GitHub webhook.

## 12. Jenkins + Maven + Docker: Practical Pipeline

This is a common interview project: building and packaging a Java Spring Boot application into a Docker image.

Assumptions:

- The repository contains a `pom.xml` and `Dockerfile`.
- Maven and Docker are available on the agent.
- Docker registry credentials are configured.
- The image name is replaced with your registry namespace.

```
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'your-user/student-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-user/student-app.git'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PASSWORD" |
                            docker login -u "$DOCKER_USER" --password-stdin
                        docker push "$IMAGE_NAME:$IMAGE_TAG"
                        docker logout
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and image push completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Review the stage logs.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}
```

This pipeline builds, tests, and pushes the image; it does not deploy it to a server. Add a separate deployment stage appropriate to your target environment.

## 13. Deploying to Another EC2 Instance

If Jenkins runs on one EC2 instance and the application must run on a second EC2 instance, Jenkins can deploy through SSH.

Typical workflow:

1. Build and test the application on the Jenkins agent.
2. Build the Docker image.
3. Push the image to a container registry.
4. Connect to the deployment server through a securely configured SSH credential or another approved deployment mechanism.
5. Pull the new image on the target server.
6. Restart or replace the application container.
7. Verify health and availability.

Avoid copying a private SSH key into the repository or hardcoding it in the Jenkinsfile. Use Jenkins Credentials, restrict SSH access, and ensure the deployment identity has only the required permissions.

## 14. Jenkins + Kubernetes

Jenkins can deploy a containerized application to Kubernetes by updating a Deployment with a new image tag.

Example deployment stage:

```
stage('Deploy to Kubernetes') {
    steps {
        sh '''
            kubectl set image deployment/student-app \
                student-app=your-user/student-app:${BUILD_NUMBER}

            kubectl rollout status deployment/student-app \
                --timeout=180s
        '''
    }
}
```

This assumes:

- The Kubernetes Deployment already exists.
- Its container name is `student-app`.
- The image is available to the cluster.
- Jenkins has a correctly configured Kubernetes context and appropriate RBAC permissions.

A successful Jenkins command does not automatically prove that the application works correctly. Include health checks or smoke tests where appropriate.

## 15. Jenkins Build Statuses

| Status    | Meaning                                                    |
| --------- | ---------------------------------------------------------- |
| Success   | The build completed successfully.                          |
| Failure   | A step failed or the pipeline explicitly failed.           |
| Unstable  | The build completed but has problems, often test failures. |
| Aborted   | The build was stopped or cancelled.                        |
| Not built | The job or stage was not executed in certain situations.   |

## 16. Common Jenkins Errors and Troubleshooting

### Error 1: `Permission denied` when running Docker

Possible cause: The Jenkins agent's operating-system user lacks access to the Docker daemon.

Check:

```
whoami
docker ps
docker info
```

Fix access through an appropriate Docker configuration or a secured agent setup. Membership in the Docker group effectively grants highly privileged access, so do not add users to it casually.

### Error 2: `mvn: command not found`

Possible cause: Maven is not installed or is unavailable in the agent's `PATH`.

Check:

```
mvn -version
java -version
```

Install the required tools or use a suitable build-agent image.

### Error 3: `Permission denied` in the Jenkins workspace

Possible cause: Files were created by another user or the workspace ownership is incorrect.

Check:

```
ls -ld "$WORKSPACE"
ls -l
```

Use the correct Jenkins service identity and repair workspace ownership carefully. Avoid running arbitrary workspace commands as root.

### Error 4: AWS credentials not found

Possible cause: The Jenkins execution environment lacks usable AWS credentials or an assigned IAM role.

Possible solutions:

- Attach an appropriate IAM role to the EC2 instance or agent.
- Use an approved AWS credential mechanism.
- Ensure the identity has the required permissions.
- Verify which operating-system user and agent actually execute the pipeline.

Never hardcode AWS access keys in a Jenkinsfile.

### Error 5: Pipeline succeeds but the application is unavailable

Possible causes:

- Deployment stage did not actually run.
- Container exited after startup.
- Incorrect port mapping.
- Missing environment variables.
- Application cannot connect to the database.
- Security group or firewall rules block traffic.

Check application logs, container status, health endpoints, and network configuration.

## 17. Important Jenkins Interview Questions

1. What is Jenkins?
2. What is CI/CD?
3. What is a Jenkins Pipeline?
4. What is a Jenkinsfile?
5. What is the difference between Declarative and Scripted Pipeline?
6. What is the difference between a Jenkins controller and an agent?
7. What is a Jenkins workspace?
8. What are Jenkins plugins?
9. How do you integrate Jenkins with GitHub?
10. What are Jenkins credentials?
11. How do you securely pass credentials to a pipeline?
12. How do you build a Docker image in Jenkins?
13. How do you deploy an application to another EC2 instance?
14. How do you deploy to Kubernetes?
15. What is the difference between Continuous Delivery and Continuous Deployment?
16. How do you troubleshoot a failed Jenkins build?
17. How do you trigger a pipeline using a webhook?
18. What are `post` conditions?
19. How do you run stages in parallel?
20. How do you secure Jenkins in production?

## 18. Your 60-Second Interview Answer

Jenkins is an open-source automation server used to automate CI/CD pipelines. In a typical DevOps workflow, a developer pushes code to GitHub, and Jenkins checks out the source code, builds the application, runs tests, and packages it.

For containerized applications, Jenkins can build a Docker image and push it to a container registry such as Docker Hub or Amazon ECR. It can then deploy the application to an EC2 instance or a Kubernetes cluster.

I use Jenkins pipelines defined in a Jenkinsfile to automate these stages. I also use Jenkins credentials to handle sensitive information securely. This automation reduces manual effort, improves consistency, and helps deliver application changes faster.

