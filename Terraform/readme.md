# Infrastructure as Code (IaC) & Terraform Basics

## 1. Introduction to IaC

* **Definition**: IaC means writing **code** (instead of clicking manually in the AWS/Azure GUI) to create, manage, and update infrastructure such as servers, networks, and databases.
* **Core Idea**: Just like you use code to build software applications, you use declarative code to define and build infrastructure.
* **Key Benefits**:
  * **Repeatable**: Generates identical environments every time.
  * **Automated**: Eliminates manual steps and human error.
  * **Version-Controlled**: Stored in Git so every change is tracked and auditable.
  * **Scalable**: Allows seamless deployment across multiple environments (Dev, Staging, Prod).

> **Example**: Instead of manually creating an EC2 instance in the AWS Management Console, you write a Terraform configuration file and execute `terraform apply`.

---

## 2. Why We Need IaC

| Aspect | Shell Script | Ansible | Terraform (IaC) |
| :--- | :--- | :--- | :--- |
| **Primary Purpose** | Task automation (e.g., software installation, file copies). | Configuration management & application deployment. | Complete infrastructure provisioning (VMs, networks, DBs). |
| **State Awareness** | ❌ No state awareness (executes commands blindly). | ⚠️ Limited state tracking. | ✅ Maintains a **state file** (`terraform.tfstate`) to track resources. |
| **Idempotency** | ❌ No — risks creating duplicate resources. | ✅ Yes — ensures target configuration state. | ✅ Yes — ensures real-world infrastructure matches code. |
| **Cloud Support** | Not inherently cloud-aware. | Cloud-compatible, but primary strength is OS configuration. | Purpose-built for multi-cloud infrastructure provisioning. |
| **Typical Use Case** | `apt-get install nginx` in a Bash script | Ansible Playbook configuring Nginx on remote servers | Terraform code creating VPC, EC2 instance, and Security Groups |

**Summary**:
* **Shell Scripts**: Imperative task automation.
* **Ansible**: Configuration management and software provisioning.
* **Terraform**: Multi-cloud infrastructure provisioning and lifecycle management.

---

## 3. Terraform Syntax Basics

Terraform configurations are written in **HashiCorp Configuration Language (HCL)**.

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}

```

* **Provider**: Specifies the target cloud provider or platform (AWS, Azure, GCP).
* **Resource**: Defines the infrastructure component to manage (EC2, VPC, S3).
* **Arguments**: Configures settings inside resources (`ami`, `instance_type`).

> Terraform is **declarative**: you declare the desired end state, and Terraform determines the steps required to achieve it.

---

## 4. Key Building Blocks in Terraform

Terraform uses several core block types to build configurations:

1. **`provider`**: Defines the target platform configuration.
```hcl
provider "aws" {
  region = "us-east-1"
}

```


2. **`resource`**: Defines infrastructure components to manage.
```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345"
  instance_type = "t2.micro"
}

```


3. **`variable`**: Accepts input values for parameterization.
```hcl
variable "region" {
  default = "us-east-1"
}

```


4. **`output`**: Displays exported values after execution.
```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}

```


5. **`module`**: Groups reusable Terraform configurations into single packages.
```hcl
module "vpc" {
  source = "./modules/vpc"
}

```


6. **`locals`**: Defines internal temporary variables and local expressions.
```hcl
locals {
  env = "dev"
}

```


7. **`data`**: Fetches read-only data from existing infrastructure outside of Terraform control.
```hcl
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["amazon"]
}

```



---

## 5. Terraform Installation & Initial Setup

### Install Terraform on Ubuntu

Create an installation script:

```bash
nano terraform-install.sh

```

Paste the following shell script:

```bash
#!/bin/bash

# Update packages
sudo apt update -y

# Install required packages
sudo apt install -y gnupg software-properties-common curl wget

# Add HashiCorp GPG key
wget -O- [https://apt.releases.hashicorp.com/gpg](https://apt.releases.hashicorp.com/gpg) | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

# Add HashiCorp repository
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] [https://apt.releases.hashicorp.com](https://apt.releases.hashicorp.com) $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

# Update package list and install Terraform
sudo apt update -y
sudo apt install -y terraform

# Verify installation
terraform version

```

Make the script executable and run it:

```bash
chmod +x terraform-install.sh
./terraform-install.sh

```

---

### Install and Configure AWS CLI v2

Download and run the official installer:

```bash
# 1. Download installer package
curl "[https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)" -o "awscliv2.zip"

# 2. Unzip installer package
unzip awscliv2.zip

# 3. Run installation script
sudo ./aws/install

# 4. Verify installation
aws --version

```

Configure your AWS credentials:

```bash
aws configure

```

Input parameters when prompted:

1. **AWS Access Key ID**: `Your Access Key`
2. **AWS Secret Access Key**: `Your Secret Key`
3. **Default region name**: `ap-south-1`
4. **Default output format**: `json`

---

### Basic EC2 Deployment Walkthrough

#### Step 1: Create `main.tf`

Open Nano:

```bash
nano main.tf

```

Paste the configuration:

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "terraform_demo" {
  ami           = "ami-01a00762f46d584a1"
  instance_type = "t3.micro"

  tags = {
    Name = "terraform-demo-server"
  }
}

```

Save and exit Nano:

* Press `Ctrl + O`, then hit `Enter` to write the file.
* Press `Ctrl + X` to exit.

#### Step 2: Deploy Infrastructure

Run standard initialization and execution lifecycle commands:

```bash
# Initialize working directory and download provider plugins
terraform init

# Preview changes
terraform plan

# Apply infrastructure changes
terraform apply -auto-approve

```

---

## 6. Terraform Security Groups & UserData (HEREDOC)

### Security Groups Overview

A **Security Group** acts as a virtual firewall for your compute instances to control inbound and outbound network traffic.

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_security_group" "web_sg" {
  name_prefix = "web-sg-"
  description = "Allow inbound HTTP and SSH traffic"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "web-sg"
  }
}

```

---

### Understanding HEREDOC in `user_data`

HEREDOC (**Here Document**) is a multi-line string syntax in HCL used to define large blocks of text or scripts directly inside configuration files.

#### Indented HEREDOC (`<<-EOF`)

Using `<<-EOF` allows you to indent script lines inside your code for improved readability without embedding leading tabs into the string output.

```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  user_data = <<-EOF
    #!/bin/bash
    yum update -y
    yum install -y httpd
    echo "Hello, World!" > /var/www/html/index.html
    systemctl start httpd
    systemctl enable httpd
  EOF

  tags = {
    Name = "web-server"
  }
}

```

#### External Script Invocation

Alternatively, reference external script files directly:

```hcl
user_data = file("index.sh")

```

---

### Security Group + HEREDOC Practical Example

```hcl
provider "aws" {
  region = "ap-south-1"
}

variable "instance_type" {
  default = "t3.micro"
}

variable "ami_id" {
  default = "ami-019715e0d74f695be"
}

resource "aws_instance" "my_ec2" {
  ami             = var.ami_id
  instance_type   = var.instance_type
  security_groups = [aws_security_group.security.name]

  user_data = base64encode(<<-EOF
      #!/bin/bash
      sudo apt update -y
      sudo apt install nginx -y 
      echo "Hello World from Terraform" > /var/www/html/index.html
      systemctl start nginx
      systemctl enable nginx
  EOF
  )

  tags = {
    Name = "MyEC2Instance"
  }
}

resource "aws_security_group" "security" {
  name        = "my-sg"
  description = "Allow SSH and HTTP traffic"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80  
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

output "public_ip" {
  value = aws_instance.my_ec2.public_ip
}

```

---

## 7. Load Balancer & Auto Scaling Group Script

```hcl
provider "aws" {
  region = "ap-south-1"
}

# ------------------------------------------------------------------------------
# Launch Templates
# ------------------------------------------------------------------------------
resource "aws_launch_template" "home_temp" {
  name          = "home-temp"
  instance_type = "t3.micro"
  image_id      = "ami-019715e0d74f695be"

  user_data = base64encode(<<-EOF
      #!/bin/bash
      apt update -y
      apt install nginx -y
      echo "<h1>WELCOME TO HOME PAGE</h1>" > /var/www/html/index.html
      systemctl start nginx
      systemctl enable nginx
  EOF
  )

  tags = {
    Name = "home-temp"
  }
}

resource "aws_launch_template" "cloth_temp" {
  name          = "cloth-temp"
  instance_type = "t3.micro"
  image_id      = "ami-019715e0d74f695be"

  user_data = base64encode(<<-EOF
      #!/bin/bash
      apt update -y
      apt install nginx -y
      mkdir -p /var/www/html/cloth
      echo "<h1>SALE SALE SALE</h1>" > /var/www/html/cloth/index.html
      systemctl start nginx
      systemctl enable nginx
  EOF
  )

  tags = {
    Name = "cloth-temp"
  }
}

# ------------------------------------------------------------------------------
# Auto Scaling Groups & Policies
# ------------------------------------------------------------------------------
resource "aws_autoscaling_group" "home_asg" {
  availability_zones = ["ap-south-1a", "ap-south-1b", "ap-south-1c"]
  desired_capacity   = 1
  max_size           = 1
  min_size           = 1
  health_check_type  = "instance"

  launch_template {
    id      = aws_launch_template.home_temp.id
    version = "$Latest"
  }
}

resource "aws_autoscaling_policy" "home_asg_policy" {
  name                   = "home-asg-policy"
  scaling_adjustment     = 1
  adjustment_type        = "ChangeInCapacity"
  cooldown               = 300
  autoscaling_group_name = aws_autoscaling_group.home_asg.name
}

resource "aws_cloudwatch_metric_alarm" "home_alarm" {
  alarm_name          = "home-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 60
  statistic           = "Average"
  threshold           = 40

  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.home_asg.name
  }

  alarm_actions = [aws_autoscaling_policy.home_asg_policy.arn]
}

resource "aws_autoscaling_group" "cloth_asg" {
  availability_zones = ["ap-south-1a", "ap-south-1b", "ap-south-1c"]
  desired_capacity   = 1
  max_size           = 1
  min_size           = 1
  health_check_type  = "ELB"

  launch_template {
    id      = aws_launch_template.cloth_temp.id
    version = "$Latest"
  }
}

resource "aws_autoscaling_policy" "cloth_asg_policy" {
  name                   = "cloth-asg-policy"
  scaling_adjustment     = 1
  adjustment_type        = "ChangeInCapacity"
  cooldown               = 300
  autoscaling_group_name = aws_autoscaling_group.cloth_asg.name
}

resource "aws_cloudwatch_metric_alarm" "cloth_alarm" {
  alarm_name          = "cloth-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 60
  statistic           = "Average"
  threshold           = 40

  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.cloth_asg.name
  }

  alarm_actions = [aws_autoscaling_policy.cloth_asg_policy.arn]
}

# ------------------------------------------------------------------------------
# Load Balancer & Target Groups
# ------------------------------------------------------------------------------
data "aws_vpc" "default" {
  default = true
}

resource "aws_lb_target_group" "home_tg" {
  name        = "home-tg"
  port        = 80
  protocol    = "HTTP"
  target_type = "instance"
  vpc_id      = data.aws_vpc.default.id
}

resource "aws_lb_target_group" "cloth_tg" {
  name        = "cloth-tg"
  port        = 80
  protocol    = "HTTP"
  target_type = "instance"
  vpc_id      = data.aws_vpc.default.id
}

resource "aws_autoscaling_attachment" "home_attach" {
  autoscaling_group_name = aws_autoscaling_group.home_asg.id
  lb_target_group_arn    = aws_lb_target_group.home_tg.arn
}

resource "aws_autoscaling_attachment" "cloth_attach" {
  autoscaling_group_name = aws_autoscaling_group.cloth_asg.id
  lb_target_group_arn    = aws_lb_target_group.cloth_tg.arn
}

resource "aws_lb" "my_alb" {
  name               = "my-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = ["sg-093e049734bea04c9", "sg-059baac33ad2c327b"]
  subnets            = ["subnet-028dbb2e5fd61f96b", "subnet-0dbce9f11e5df5e87"]
}

resource "aws_lb_listener" "my_list" {
  load_balancer_arn = aws_lb.my_alb.arn
  port              = "80"
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.home_tg.arn
  }
}

resource "aws_lb_listener_rule" "cloth_rule" {
  listener_arn = aws_lb_listener.my_list.arn
  priority     = 60

  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.cloth_tg.arn
  }

  condition {
    path_pattern {
      values = ["/cloth/*"]
    }
  }
}

```

---

## 8. Terraform Modules

### What is a Terraform Module?

A **Terraform Module** is a container for multiple resources configured together. It allows you to organize, encapsulate, and reuse infrastructure definitions across environments.

```text
Without Modules:                        With Modules:
Project A ── EC2 Resource                           EC2 Module
Project B ── EC2 Resource                                │
Project C ── EC2 Resource               ┌────────────────┼────────────────┐
                                     Dev EC2          Test EC2        Prod EC2

```

---

### Recommended Module Project Structure

```text
project/
├── main.tf             # Root module infrastructure configuration
├── variables.tf        # Root input variables
├── terraform.tfvars    # Values assigned to variables
├── outputs.tf          # Root output values
└── modules/
    └── ec2/            # Child module folder
        ├── main.tf     # Child resource configuration
        ├── variables.tf# Child input definitions
        └── outputs.tf  # Child exported parameters

```

---

### Practical Walkthrough: Building an EC2 Module

#### 1. Define Child Module Files (`modules/ec2/`)

**`modules/ec2/main.tf`**

```hcl
resource "aws_instance" "this" {
  ami           = var.ami
  instance_type = var.instance_type

  tags = {
    Name = var.instance_name
  }
}

```

**`modules/ec2/variables.tf`**

```hcl
variable "ami" {
  type = string
}

variable "instance_type" {
  type = string
}

variable "instance_name" {
  type = string
}

```

**`modules/ec2/outputs.tf`**

```hcl
output "instance_id" {
  value = aws_instance.this.id
}

output "public_ip" {
  value = aws_instance.this.public_ip
}

```

---

#### 2. Call Module from Root Directory

**`main.tf`**

```hcl
provider "aws" {
  region = "ap-south-1"
}

module "my_ec2" {
  source        = "./modules/ec2"
  ami           = var.ami
  instance_type = var.instance_type
  instance_name = var.instance_name
}

```

**`variables.tf`**

```hcl
variable "ami" {
  type = string
}

variable "instance_type" {
  type = string
}

variable "instance_name" {
  type = string
}

```

**`terraform.tfvars`**

```hcl
ami           = "ami-019715e0d74f695be"
instance_type = "t2.micro"
instance_name = "Dev-Server"

```

**`outputs.tf`**

```hcl
output "instance_id" {
  value = module.my_ec2.instance_id
}

output "public_ip" {
  value = module.my_ec2.public_ip
}

```

---

### Module Execution Rules

Run all execution commands exclusively from the **Root Directory**:

```bash
cd project/
terraform init
terraform plan
terraform apply

```

> **Warning**: Never execute `terraform apply` directly inside child module directories (`/modules/ec2/`).

---

## 9. Multi-Environment Deployments via `.tfvars`

Keep infrastructure code DRY (Don't Repeat Yourself) by decoupling resource blocks from environment settings using dedicated `.tfvars` configuration files.

### Folder Structure

```text
ec2-multi-env/
├── main.tf
├── variables.tf
└── env/
    ├── dev.tfvars
    ├── stage.tfvars
    └── prod.tfvars

```

### Configuration Files

**`main.tf`**

```hcl
provider "aws" {
  region = var.aws_region
}

resource "aws_instance" "my_ec2" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name = "ec2-${var.environment}"
    Env  = var.environment
  }
}

```

**`variables.tf`**

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
}

variable "environment" {
  description = "Environment name (dev, stage, prod)"
  type        = string
}

variable "aws_region" {
  description = "AWS region"
  type        = string
}

variable "ami_id" {
  description = "AMI ID"
  type        = string
}

```

**`env/dev.tfvars`**

```hcl
environment   = "dev"
aws_region    = "us-east-1"
ami_id        = "ami-08c40ec9ead489470"
instance_type = "t2.micro"

```

**`env/stage.tfvars`**

```hcl
environment   = "stage"
aws_region    = "us-east-1"
ami_id        = "ami-08c40ec9ead489470"
instance_type = "t3.small"

```

**`env/prod.tfvars`**

```hcl
environment   = "prod"
aws_region    = "us-east-1"
ami_id        = "ami-08c40ec9ead489470"
instance_type = "t3.medium"

```

### Deployment Commands

```bash
# Initialize working directory
terraform init

# Deploy Development
terraform apply -var-file="env/dev.tfvars"

# Deploy Staging
terraform apply -var-file="env/stage.tfvars"

# Deploy Production
terraform apply -var-file="env/prod.tfvars"

```

---

## 10. Remote State Storage & Locking

Storing state files (`terraform.tfstate`) centrally in remote storage enables team collaboration, ensures secure encryption at rest, and provides state locking mechanisms to prevent concurrent modifications.

```hcl
terraform {
  backend "s3" {
    bucket         = "your-terraform-state-bucket"
    key            = "global/s3/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-locks" # Enables state locking
  }
}

```

---

## 11. Terraform Workspaces

Workspaces allow you to manage isolated state files using a single directory of Terraform configuration files.

### Workspace Setup Walkthrough

```bash
# 1. Create project directory
mkdir terraform-workspace-demo
cd terraform-workspace-demo
touch main.tf

```

Add the following to **`main.tf`**:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "example-bucket-${terraform.workspace}"

  tags = {
    Name = "workspace-demo"
  }
}

```

Execute Workspace commands:

```bash
# Initialize directory
terraform init

# List available workspaces (* indicates current active workspace)
terraform workspace list

# Create new isolated workspaces
terraform workspace new dev
terraform workspace new stage
terraform workspace new prod

# Select an active workspace
terraform workspace select dev
terraform apply

```

---

## 12. Architectural Comparison: Modules vs `.tfvars` vs Workspaces

| Aspect | Modules | `.tfvars` Files | Workspaces |
| --- | --- | --- | --- |
| **Primary Concept** | Code organization & structure | Input value parameterization | State file isolation |
| **Primary Purpose** | Avoid code duplication | Adjust variables per environment | Isolate deployment state |
| **Modifies Code** | Yes | No | No |
| **Modifies Variables** | No | Yes | No |
| **Modifies State** | No | No | Yes |
| **Best Used For** | Standardizing infrastructure (VPCs, Clusters) | Managing config settings per environment | Isolating state across ephemeral environments |

---

## 13. Iteration Constructs: Loops in Terraform

### 1. `count`

Creates identical instances based on a specified numeric value.

```hcl
resource "aws_instance" "example" {
  count         = 3
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}

```

Reference instances using index notation: `aws_instance.example[0]`

---

### 2. `for_each`

Iterates over set or map structures to build items with distinct properties.

```hcl
resource "aws_s3_bucket" "example" {
  for_each = {
    dev  = "dev-bucket-unique-1"
    prod = "prod-bucket-unique-2"
  }

  bucket = each.value
}

```

Reference resources using keys: `aws_s3_bucket.example["dev"]`

---

### 3. `for` Expressions

Transforms or filters existing collection data.

```hcl
variable "names" {
  default = ["Alice", "Bob", "Charlie"]
}

# Transform elements to uppercase
output "uppercase_names" {
  value = [for name in var.names : upper(name)]
}

# Filter elements based on length logic
output "filtered_names" {
  value = [for name in var.names : name if length(name) > 3]
}

```

---

### Iteration Practical Example

```hcl
provider "aws" {
  region = "us-east-1"   
}

variable "ami_ids" {
  type    = list(string)
  default = ["ami-0c55b159cbfafe1f0", "ami-00ca32bbc84273381", "ami-0fd3ac4abb734302a"]
}

resource "aws_instance" "my_ec2" {
  for_each      = toset(var.ami_ids)
  ami           = each.value
  instance_type = "t3.micro"

  tags = {
    Name = "MyFirstEC2-${each.key}"
  }
}

output "public_ip" {
  value = { for instance in aws_instance.my_ec2 : instance.id => instance.arn }
}

```

---

## 14. Essential Operations & CLI Commands

### Taint Command

Forces a specific resource to be destroyed and recreated on the next `apply`.

```bash
# Modern CLI syntax (Terraform 0.15+)
terraform apply -replace="aws_instance.my_instance"

# Legacy CLI syntax
terraform taint aws_instance.my_instance

```

---

### Import Command

Imports existing infrastructure created outside of Terraform into state management.

```bash
terraform import aws_instance.my_instance i-0abcd1234efgh5678

```

---

### Targeted Destruction

Destroys specifically targeted individual resources without destroying the entire state stack.

```bash
terraform destroy -target=aws_instance.my_instance

```

---

## 15. Terraform Provisioners (`local-exec`, `file`, `remote-exec`)

> **Note**: Provisioners should be used as a last resort. Use `user_data` or configuration management tools (such as Ansible) whenever possible.

### Provisioner Types Summary

| Provisioner | Execution Location | Primary Purpose |
| --- | --- | --- |
| **`local-exec`** | Local machine running Terraform | Triggers local scripts or triggers automated pipelines |
| **`file`** | Local machine ➔ Remote instance | Copies local files or directories to target servers |
| **`remote-exec`** | Remote target server | Executes inline terminal commands inside provisioned instances |

---

### Comprehensive Provisioners Practical

#### 1. Setup Project Directory & Files

```bash
mkdir provisioner-demo
cd provisioner-demo
touch main.tf hello.txt

```

Add initial content to **`hello.txt`**:

```text
Hello from Terraform Provisioner

```

Ensure your private key file (`terraform-key.pem`) is present inside the project folder:

```bash
chmod 400 terraform-key.pem

```

---

#### 2. `main.tf` Configuration

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "web" {
  ami                    = "ami-019715e0d74f695be"
  instance_type          = "t2.micro"
  key_name               = "terraform-key"
  vpc_security_group_ids = ["sg-xxxxxxxx"] # Replace with valid SG ID allowing Port 22 SSH

  tags = {
    Name = "Provisioner-Demo"
  }

  # Connection settings required for file and remote-exec provisioners
  connection {
    type        = "ssh"
    user        = "ec2-user"
    private_key = file("terraform-key.pem")
    host        = self.public_ip
  }

  # 1. Local Provisioner (Runs on local machine)
  provisioner "local-exec" {
    command = "echo EC2 Created Successfully > output.txt"
  }

  # 2. File Provisioner (Uploads local file to target instance)
  provisioner "file" {
    source      = "hello.txt"
    destination = "/home/ec2-user/hello.txt"
  }

  # 3. Remote Provisioner (Executes commands on target server)
  provisioner "remote-exec" {
    inline = [
      "echo 'Reading File'",
      "cat /home/ec2-user/hello.txt",
      "touch demo.txt",
      "echo 'Provisioner Completed' > demo.txt"
    ]
  }
}

output "public_ip" {
  value = aws_instance.web.public_ip
}

```

---

#### 3. Execute and Verify Deployment

```bash
terraform init
terraform validate
terraform apply -auto-approve

```

Verify outcomes:

* **Local Machine Verification**:
```bash
cat output.txt
# Output: EC2 Created Successfully

```


* **Remote Machine Verification**:
```bash
ssh -i terraform-key.pem ec2-user@<PUBLIC-IP>
ls -la
cat hello.txt
cat demo.txt

```



---

## 16. EKS Cluster Provisioning via Terraform

```hcl
# IAM Role for EKS Control Plane
resource "aws_iam_role" "cluster" {
  name = "eks-cluster-example"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "sts:AssumeRole",
          "sts:TagSession"
        ]
        Effect = "Allow"
        Principal = {
          Service = "eks.amazonaws.com"
        }
      }
    ]
  })
}

resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSClusterPolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
  role       = aws_iam_role.cluster.name
}

# Fetch Default VPC & Subnets
data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}

# EKS Cluster Resource
resource "aws_eks_cluster" "cluster" {
  name     = "cluster"
  role_arn = aws_iam_role.cluster.arn

  access_config {
    authentication_mode = "API"
  }

  vpc_config {
    subnet_ids = data.aws_subnets.default.ids
  }

  depends_on = [
    aws_iam_role_policy_attachment.cluster_AmazonEKSClusterPolicy
  ]
}

```
