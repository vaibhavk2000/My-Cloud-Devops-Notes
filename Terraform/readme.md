# Infrastructure as Code (IAC) & Terraform Basics

## 1. Introduction to IAC
- **Definition**: IAC means writing **code** (instead of clicking manually in AWS/Azure GUI) to create and manage infrastructure like servers, networks, databases, etc.  
- **Idea**: Just like you use code to build an app, you use code to build infrastructure.  
- **Benefits**:  
  - Repeatable → Same setup every time.  
  - Automated → Saves manual effort.  
  - Version-controlled → Stored in Git, so changes are trackable.  
  - Scalable → Easy to deploy infra across multiple environments (Dev, Test, Prod).  

👉 Example: Instead of manually creating an EC2 in AWS Console, you write a Terraform file and just run `terraform apply`.

---

## 2. Why we need IAC (Difference between Shell Script, Ansible, and IAC tools like Terraform)

| **Aspect** | **Shell Script** | **Ansible** | **IAC Tool (Terraform)** |
|------------|------------------|-------------|--------------------------|
| **Purpose** | Automates tasks (e.g., install software, copy files). | Config management + automation. | Full infra provisioning (VMs, networks, DBs). |
| **State awareness** | No state awareness → runs blindly. | Limited state tracking. | Maintains **state file** → knows what exists and what needs change. |
| **Idempotency** | ❌ No → may create duplicates. | ✅ Yes → ensures final state. | ✅ Yes → ensures infrastructure matches code. |
| **Cloud support** | Not cloud-focused. | Supports cloud but mainly for config mgmt. | Designed for multi-cloud infra provisioning. |
| **Example** | Bash script: `apt-get install nginx` | Ansible Playbook to install Nginx | Terraform code to create EC2 + attach security group + install Nginx |

👉 In short:  
- **Shell Script** = manual automation.  
- **Ansible** = config management & software deployment.  
- **Terraform (IAC)** = provisioning complete infra in a controlled, declarative way.  

---


## 3. Terraform Language (Basic Syntax)
Terraform files are written in **HCL (HashiCorp Configuration Language)**.  

Example:
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}
```

- **Provider** → Defines which cloud/service you are using (AWS, Azure, GCP).  
- **Resource** → Defines what infra you want (EC2, VPC, S3, etc.).  
- **Arguments** → Settings inside resources (`ami`, `instance_type`).  

👉 It’s **declarative** → you say *what you want*, Terraform figures out *how to do it*.  

---

## 4. Enlist the Blocks used in Terraform Language

Terraform has multiple **blocks** (building units):  

1. **provider** → Defines the provider (AWS, Azure, etc.)  
   ```hcl
   provider "aws" {
     region = "us-east-1"
   }
   ```

2. **resource** → Defines infrastructure resources.  
   ```hcl
   resource "aws_instance" "example" {
     ami           = "ami-12345"
     instance_type = "t2.micro"
   }
   ```

3. **variable** → Input values (like parameters).  
   ```hcl
   variable "region" {
     default = "us-east-1"
   }
   ```

4. **output** → Shows values after deployment.  
   ```hcl
   output "instance_ip" {
     value = aws_instance.example.public_ip
   }
   ```

5. **module** → Group of Terraform files reused as a package.  
   ```hcl
   module "vpc" {
     source = "./modules/vpc"
   }
   ```

6. **locals** → Define local variables.  
   ```hcl
   locals {
     env = "dev"
   }
   ```

7. **data** → Fetch existing info (e.g., latest AMI).  
   ```hcl
   data "aws_ami" "latest" {
     most_recent = true
     owners      = ["amazon"]
   }
   ```

---
### Terraform Instalation
# Install Terraform on Ubuntu Using a Single Script

## Create the Script

```bash
nano terraform-install.sh
```

Paste the following script into the file:

```bash
#!/bin/bash

# Update packages
sudo apt update -y

# Install required packages
sudo apt install -y gnupg software-properties-common curl wget

# Add HashiCorp GPG key
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

# Add HashiCorp repository
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

# Update package list
sudo apt update -y

# Install Terraform
sudo apt install -y terraform

# Verify installation
terraform version
```

## Make the Script Executable

```bash
chmod +x terraform-install.sh
```

## Run the Script

```bash
./terraform-install.sh
```


```
### Terraform file that creates an EC2 instance on AWS
```hcl
# Define the AWS provider
provider "aws" {
  region = "us-east-1"   
}

# Create an EC2 instance
resource "aws_instance" "my_ec2" {
  ami           = "ami-0c55b159cbfafe1f0" 
  instance_type = "t2.micro"              

  tags = {
    Name = "MyFirstEC2"
  }
}

```
Terraform Script to Deploy Security Group with HEREDOC in UserData

This guide covers the deployment of a Security Group using Terraform and explains the HEREDOC concept in UserData along with the key blocks in the script.

---

## 1. Introduction to Security Groups
A **Security Group** acts as a virtual firewall for your instance to control inbound and outbound traffic. Terraform allows you to define and manage Security Groups using Infrastructure as Code.

---

## 2. Terraform Script for Security Group

### Script
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

## 3. HEREDOC in UserData

### What is HEREDOC?
HEREDOC (**Here Document**) is a multi-line string syntax in Terraform used to define large blocks of text or commands. It is often utilized in `UserData` to pass startup scripts to cloud instances.

### Example with UserData
Below is an example of using HEREDOC within an EC2 instance resource:

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

### Another Example:

```
user_data = <<EOF
  ${file("index.sh")}
  EOF
```

### HEREDOC Syntax
- **`<<-EOF`**: Begins the HEREDOC. The `-` allows indentation.
- **Content**: The script or text.
- **`EOF`**: Ends the HEREDOC block.

---

## 4. Key Blocks in the Terraform Script

### Provider Block
The `provider` block specifies the cloud provider to manage resources.

#### Example:
```hcl
provider "aws" {
  region = "us-east-1"
}
```
- **`region`**: Defines the AWS region for resource deployment.

### Resource Block
The `resource` block defines the actual infrastructure components.

#### Example:
```hcl
resource "aws_security_group" "web_sg" {
  name_prefix = "web-sg-"
  description = "Allow inbound HTTP and SSH traffic"
}
```
- **`name_prefix`**: Prefix for the Security Group name.
- **`ingress`/`egress`**: Rules for inbound and outbound traffic.

### Variable Block
The `variable` block is used to parameterize values, making the script reusable.

#### Example:
```hcl
variable "region" {
  default = "us-east-1"
}
```
- **`default`**: Specifies a default value.

### Data Block
The `data` block retrieves existing resources.

#### Example:
```hcl
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["self"]
}
```
- **`most_recent`**: Fetches the latest AMI.

### Output Block
The `output` block displays resource attributes after execution.

#### Example:
```hcl
output "security_group_id" {
  value = aws_security_group.web_sg.id
}
```
- **`value`**: Specifies the attribute to output.

---

## 5. Applying the Script

### Steps:
1. Initialize Terraform:
   ```bash
   terraform init
   ```

2. Validate the script:
   ```bash
   terraform validate
   ```

3. Plan the execution:
   ```bash
   terraform plan
   ```

4. Apply the changes:
   ```bash
   terraform apply
   ```

5. Verify the Security Group in the AWS Console.
---
## Security group + Heredoc Hands-on .tf file
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

resource "aws_instance" "my-ec2" {
  ami = var.ami_id
  instance_type = var.instance_type
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
  name = "my-sg"
  description = "Allow SSH and HTTP traffic"

  ingress {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 80
    to_port = 80  
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
  }
}


output "public_ip" {
  value = aws_instance.my-ec2.public_ip
}
```
# LoadBalancer, AutoScalingGroup Script
```hcl
provider "aws" {
  region = "ap-south-1" 
}

resource "aws_launch_template" "home-temp" {
    name = "home-temp"
    instance_type =  "t3.micro"
    image_id =  "ami-019715e0d74f695be"
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

resource "aws_launch_template" "cloth-temp" {
    name = "cloth-temp"
    instance_type =  "t3.micro"
    image_id =  "ami-019715e0d74f695be"
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


resource "aws_autoscaling_group" "home-asg" {
    availability_zones = ["ap-south-1a", "ap-south-1b","ap-south-1c"]
    desired_capacity = 1
    max_size = 1
    min_size = 1
    health_check_type = "instance"

    launch_template {
        id = aws_launch_template.home-temp.id
        version = "$Latest"
    }
}

resource "aws_autoscaling_policy" "home-asg-policy" {
    name = "home-asg-policy"
    scaling_adjustment = 1
    adjustment_type = "ChangeInCapacity"
    cooldown = 300
    autoscaling_group_name = aws_autoscaling_group.home-asg.name

}

resource "aws_cloudwatch_metric_alarm" "home-alarm" {
    alarm_name = "home-alarm"
    comparison_operator = "GreaterThanThreshold"
    evaluation_periods  = 2
    metric_name         = "CPUUtilization"
    namespace           = "AWS/EC2"
    period              = 60
    statistic           = "Average"
    threshold           = 40

    dimensions = {
        AutoScalingGroupName = aws_autoscaling_group.home-asg.name
    }

    alarm_actions = [aws_autoscaling_policy.home-asg-policy.arn]
}

resource "aws_autoscaling_group" "cloth-asg" {
    availability_zones = ["ap-south-1a", "ap-south-1b","ap-south-1c"]
    desired_capacity = 1
    max_size = 1
    min_size = 1
    health_check_type = "ELB"
    launch_template {
        id = aws_launch_template.cloth-temp.id
        version = "$Latest"
    }
}

resource "aws_autoscaling_policy" "cloth-asg-policy" {
    name = "cloth-asg-policy"
    scaling_adjustment = 1
    adjustment_type = "ChangeInCapacity"
    cooldown = 300
    autoscaling_group_name = aws_autoscaling_group.cloth-asg.name

}

resource "aws_cloudwatch_metric_alarm" "cloth-alarm" {
    alarm_name = "cloth-alarm"
    comparison_operator = "GreaterThanThreshold"
    evaluation_periods  = 2
    metric_name         = "CPUUtilization"
    namespace           = "AWS/EC2"
    period              = 60
    statistic           = "Average"
    threshold           = 40

    dimensions = {
        AutoScalingGroupName = aws_autoscaling_group.cloth-asg.name
    }

    alarm_actions = [aws_autoscaling_policy.cloth-asg-policy.arn]
}

data "aws_vpc" "default" {
    default = true
}

resource "aws_lb_target_group" "home-tg" {
    name = "home-tg"
    port = 80
    protocol = "HTTP"
    target_type= "instance"
    vpc_id = data.aws_vpc.default.id
}

resource "aws_lb_target_group" "cloth-tg" {
    name = "cloth-tg"
    port = 80
    protocol = "HTTP"
    target_type = "instance"
    vpc_id = data.aws_vpc.default.id
}

resource "aws_autoscaling_attachment" "home-attach" {
    autoscaling_group_name = aws_autoscaling_group.home-asg.id
    lb_target_group_arn = aws_lb_target_group.home-tg.arn
}

resource "aws_autoscaling_attachment" "cloth-attach" {
    autoscaling_group_name = aws_autoscaling_group.cloth-asg.id
    lb_target_group_arn = aws_lb_target_group.cloth-tg.arn
}


resource "aws_lb" "my-alb" {
    name = "my-alb"
    internal = false
    load_balancer_type = "application"
    security_groups = ["sg-093e049734bea04c9", "sg-059baac33ad2c327b"]
    subnets = ["subnet-028dbb2e5fd61f96b", "subnet-0dbce9f11e5df5e87"]
}

resource "aws_lb_listener" "my-list" {
    load_balancer_arn = aws_lb.my-alb.arn
    port = "80"
    protocol = "HTTP"
    default_action {
        type = "forward"
        target_group_arn = aws_lb_target_group.home-tg.arn
    }
}

resource "aws_lb_listener_rule" "cloth-rule" {
    listener_arn = aws_lb_listener.my-list.arn
    priority = 60
    action {
        type = "forward"
        target_group_arn = aws_lb_target_group.cloth-tg.arn
    }
    condition {
        path_pattern {
            values = ["/cloth/*"]
        }
    }
}
```
## 📌 1. What is a Terraform Module?

# Terraform Modules - EC2 Practical 

## 📖 What is a Terraform Module?

A **Terraform Module** is a **reusable collection of Terraform configuration files** that performs a specific task.

Instead of writing the same infrastructure code multiple times, we create it **once** inside a module and reuse it wherever required.

> **Definition:** A Terraform Module is a reusable container of Terraform resources.

---

# 🤔 Why Do We Need Modules?

Imagine your company needs EC2 instances for three different environments:

- Development
- Testing
- Production

## Without Modules

You write the same EC2 code three times.

```text
Project A
└── EC2 Resource

Project B
└── EC2 Resource

Project C
└── EC2 Resource
```

### Problems

- ❌ Duplicate Code
- ❌ Difficult Maintenance
- ❌ Higher Chance of Errors
- ❌ Time Consuming

---

## With Modules

Create the EC2 code once and reuse it.

```text
                 EC2 Module
                      │
        ┌─────────────┼─────────────┐
        │             │             │
     Dev EC2      Test EC2      Prod EC2
```

### Benefits

- ✅ Reusable Code
- ✅ Easy Maintenance
- ✅ Cleaner Project Structure
- ✅ Faster Development
- ✅ Standardized Infrastructure

---

# ☕ Real-Life Example

Imagine a **Coffee Machine**.

The machine already knows how to make coffee.

You simply provide:

- Coffee Type
- Sugar
- Size

The machine prepares the coffee.

Similarly, a Terraform module already knows **how to create an EC2 instance**.

You only provide:

- AMI ID
- Instance Type
- Instance Name

The module creates the EC2 instance.

---

# 📁 Project Structure

```text
project/
│
├── main.tf
├── variables.tf
├── terraform.tfvars
├── outputs.tf
│
└── modules/
    └── ec2/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

# 🚀 Practical - Create an EC2 Module

## Step 1: Create the Module

Create the following directory:

```text
modules/
└── ec2/
```

---

## modules/ec2/main.tf

```hcl
resource "aws_instance" "this" {

  ami           = var.ami
  instance_type = var.instance_type

  tags = {
    Name = var.instance_name
  }

}
```

### Explanation

- Creates an EC2 instance.
- Uses variables instead of hardcoded values.
- No provider block is required here.

---

## modules/ec2/variables.tf

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

### Explanation

These variables receive values from the Root Module.

---

## modules/ec2/outputs.tf

```hcl
output "instance_id" {
  value = aws_instance.this.id
}

output "public_ip" {
  value = aws_instance.this.public_ip
}
```

### Explanation

Outputs return values back to the Root Module.

---

# 📞 Step 2: Call the Module

Now move to the Root Module.

---

## main.tf

```hcl
provider "aws" {
  region = "ap-south-1"
}

module "my_ec2" {

  source = "./modules/ec2"

  ami            = var.ami
  instance_type  = var.instance_type
  instance_name  = var.instance_name

}
```

### Explanation

- Configure the AWS provider.
- Call the EC2 module.
- Pass required input values.

---

## variables.tf

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

---

## terraform.tfvars

```hcl
ami            = "ami-019715e0d74f695be"
instance_type  = "t2.micro"
instance_name  = "Dev-Server"
```

---

## outputs.tf

```hcl
output "instance_id" {
  value = module.my_ec2.instance_id
}

output "public_ip" {
  value = module.my_ec2.public_ip
}
```

---

# 🔄 How Terraform Works

```text
terraform apply
        │
        ▼
Root Module
(main.tf)
        │
        ▼
Calls EC2 Module
        │
        ▼
modules/ec2/main.tf
        │
        ▼
Creates EC2 Instance
        │
        ▼
Returns Outputs
```

---

# ▶️ Execution

Run Terraform commands **only from the Root Project**.

```bash
cd project

terraform init

terraform plan

terraform apply
```

❌ Never execute Terraform inside:

```text
modules/ec2/
```

because it is only a reusable module.

---

# ♻️ Reusing the Module

Need another EC2 instance?

Simply call the same module again.

```hcl
module "test_ec2" {

  source = "./modules/ec2"

  ami            = var.ami
  instance_type  = "t2.micro"
  instance_name  = "Test-Server"

}
```

Need a Production Server?

```hcl
module "prod_ec2" {

  source = "./modules/ec2"

  ami            = var.ami
  instance_type  = "t3.medium"
  instance_name  = "Prod-Server"

}
```

Terraform will create:

- Dev Server
- Test Server
- Production Server

using the **same EC2 module**.

---

# 📌 Important Notes

### A Module Should Contain

```text
main.tf
variables.tf
outputs.tf
```

### A Module Should NOT Contain

```text
terraform.tfstate
terraform.tfvars
.terraform/
terraform.lock.hcl
```

These files belong only in the **Root Module**.

---




---
# Multi-Environment Terraform (.tfvars)
You keep one set of Terraform code (resources, modules, variables) and switch values (CIDRs, sizes, tags, etc.) via per-environment .tfvars files. That way you don’t duplicate code—only the inputs change.
```bash
ec2-multi-env/
├─ main.tf
├─ variables.tf
├─ env/
│  ├─ dev.tfvars
│  ├─ stage.tfvars
│  └─ prod.tfvars
```

## create project root
```sh
mkdir ec2-multi-env
cd ec2-multi-env
```
## create terraform files
```sh
touch main.tf variables.tf
```
## Create Environment Directory
```sh
mkdir env
```
## Create Environment .tfvars Files
```sh
touch env/dev.tfvars env/stage.tfvars env/prod.tfvars
```
---
### main.tf
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
### variables.tf
```
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
## Environment-specific .tfvars
### env/dev.tfvars
```hcl
environment   = "dev"
aws_region    = "us-east-1"
ami_id        = "ami-08c40ec9ead489470"
instance_type = "t2.micro"
```
### env/stage.tfvars
```hcl
environment   = "stage"
aws_region    = "us-east-1"
ami_id        = "ami-08c40ec9ead489470"
instance_type = "t3.small"
```
### env/prod.tfvars
```hcl
environment   = "prod"
aws_region    = "us-east-1"
ami_id        = "ami-08c40ec9ead489470"
instance_type = "t3.medium"
```
## Terraform Commands
```sh
terraform init
```
### deploy DEV
```sh
terraform apply -var-file="env/dev.tfvars"
```
### deploy stage
```sh
terraform apply -var-file="env/stage.tfvars"
```
### deploy prod
```sh
terraform apply -var-file="env/prod.tfvars"
```
---
# Remote State Storage 

## Storing `terraform.tfstate` on a Remote Location

### Why Store State Remotely?
- Storing the state file remotely enables team collaboration.
- Provides state locking to prevent conflicts during simultaneous updates.
- Secures sensitive data stored in the state file.

### Example: AWS S3 Backend Configuration
Use the following configuration to store your Terraform state in an S3 bucket.

```hcl
terraform {
  backend "s3" {
    bucket         = "your-terraform-state-bucket"  # Replace with your bucket name
    key            = "terraform.tfstate"
    region         = "us-east-1"  # Replace with your AWS region
    encrypt        = true
  }
}
```
# Terraform Workspace 

##  Objective
To understand how **Terraform Workspaces** work by creating the **same resource** in **different workspaces**, where only the **state file and resource name change**.

---

##  Concept Recap 

- Terraform workspaces allow you to use **one Terraform configuration**
- Each workspace has its **own state file**
- Resources are **separate**, even though code is the same
- `terraform.workspace` gives the **current workspace name**

---

## Step 1: Create Project Directory

```bash
mkdir terraform-workspace-demo
cd terraform-workspace-demo
```
## Step 2: Create Terraform File
```sh
touch main.tf
```
## Step 3: Add Terraform Configuration (main.tf)
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "example-bucket-${terraform.workspace}"
  acl    = "private"

  tags = {
    Name = "workspace-demo"
  }
}
```
Explanation - 
- ${terraform.workspace} automatically picks the active workspace name
- Each workspace creates a different S3 bucket
- Same code → different bucket names

## Step 4: Initialize Terraform
```sh
terraform init
```
## Step 5: Check Existing Workspaces
```sh
terraform workspace list
```
## Step 6: Create New Workspaces
```sh
terraform workspace new dev
terraform workspace new stage
terraform workspace new prod
```
## Step 7: Switch Between Workspaces
```sh
terraform workspace select <workspace_name>
```
## terraform workspace select <workspace_name>
```sh
terraform workspace select dev
terraform apply
```
---
# Difference Between Terraform Modules, .tfvars, and Workspaces

| Aspect | Modules | .tfvars | Workspaces |
|------|--------|---------|-----------|
| What it is | A way to organize and reuse Terraform code | A file used to provide variable values | A mechanism to maintain separate state files |
| Primary purpose | Avoid code duplication | Change configuration values without changing code | Isolate Terraform state |
| Affects Terraform code | Yes | No | No |
| Affects variable values | No | Yes | No |
| Affects state file | No | No | Yes |
| Reusability | High – same module can be used multiple times | Not reusable code, only values | Not reusable, only state separation |
| Typical use case | Large or repeated infrastructure components | Different configurations for dev, stage, prod | Logical separation of infrastructure states |
| Common examples | VPC module, EC2 module, RDS module | instance_type, region, CIDR blocks | default, dev, test |
| Recommended for production | Yes | Yes | Limited (use carefully) |
| Learning curve | Medium | Easy | Easy to Medium |
| Mental model | Code structure | Configuration values | State management |

## One-Line Summary

- Modules define how infrastructure is built.
- .tfvars define what values are used.
- Workspaces define where Terraform stores its state.
---
## Terraform Loops
Terraform provides powerful constructs for iterating over collections like `list` and `map`. The primary looping mechanisms are `count`, `for_each`, and `for`.

### 1. `count`
- **Definition**: The `count` parameter allows you to specify how many instances of a resource to create.
- **Usage**: Works well for creating identical resources.

#### Example:
```hcl
resource "aws_instance" "example" {
  count         = 3
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}
```
In this example, three EC2 instances are created.

#### Accessing Instances:
```hcl
aws_instance.example[0]  # First instance
aws_instance.example[1]  # Second instance
aws_instance.example[2]  # Third instance
```

### 2. `for_each`
- **Definition**: The `for_each` meta-argument allows iterating over `map` or `set` types to create resources with distinct properties.
- **Usage**: Useful when resource properties vary.

#### Example:
```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_s3_bucket" "example" {
  for_each = {
    dev  = "dev-bucket-unique-1"
    prod = "prod-bucket-unique-2"
  }

  bucket = each.value
}



```
This creates two S3 buckets: `dev-bucket` and `prod-bucket`.

#### Accessing Instances:
```hcl
aws_s3_bucket.example["dev"]  # Dev bucket
aws_s3_bucket.example["prod"] # Prod bucket
```

### 3. `for`
- **Definition**: The `for` expression is used to transform or filter collections.
- **Usage**: Commonly used in variables and outputs.

#### Example:
```hcl
variable "names" {
  default = ["Alice", "Bob", "Charlie"]
}

output "uppercase_names" {
  value = [for name in var.names : upper(name)]
}
```
This outputs the names in uppercase: `["ALICE", "BOB", "CHARLIE"]`.

#### Filtering with `for`:
```hcl
output "filtered_names" {
  value = [for name in var.names : name if length(name) > 3]
}
```
This filters names longer than three characters.

---

## Comparison Table
| Feature      | `count`                  | `for_each`                  | `for`                    |
|--------------|--------------------------|-----------------------------|--------------------------|
| Input Type   | Number                   | Map or Set                  | List, Map, or Set        |
| Use Case     | Create identical items   | Create unique items         | Transform or filter data |
| Example      | EC2 instances            | S3 buckets with unique IDs  | Modify list of names     |

---
## Example
```hcl
# Define the AWS provider
provider "aws" {
  region = "us-east-1"   
}

# Create an EC2 instance
resource "aws_instance" "my_ec2" {
  for_each = toset(var.ami_ids)
  ami = each.value
  instance_type = "t3.micro"

#  count = 3
  tags = {
    Name = "MyFirstEC2"
  }
}

variable "ami_ids" {
    default = ["ami-0c55b159cbfafe1f0", "ami-00ca32bbc84273381", "ami-0fd3ac4abb734302a"]
    type = list(string)
}

output "public_ip" {
    value = { for instance in aws_instance.my_ec2: instance.id => instance.arn }
}
```
# Terraform Commands and Provisioners

## Terraform Commands

### 1. **Taint Command**
The `taint` command marks a resource for recreation during the next `terraform apply`. This is useful when a specific resource needs to be replaced without altering the rest of the infrastructure.

#### **Syntax:**
```bash
terraform taint <resource_name>
```

#### **Example:**
```bash
terraform taint aws_instance.my_instance
```
This marks the `aws_instance.my_instance` resource for recreation.

---

### 2. **Import Command**
The `import` command allows importing existing infrastructure resources into Terraform state. This is helpful when managing resources created outside of Terraform.

#### **Syntax:**
```bash
terraform import <resource_type>.<resource_name> <resource_id>
```

#### **Example:**
```bash
terraform import aws_instance.my_instance i-0abcd1234efgh5678
```
This imports the AWS EC2 instance with ID `i-0abcd1234efgh5678` into Terraform as `aws_instance.my_instance`.

---

### 3. **Destroy Command**
The `destroy` command removes all resources defined in the configuration.

#### **Targeted Destroy (-t)**
You can destroy specific resources using the `-target` flag.

#### **Syntax:**
```bash
terraform destroy -target=<resource_type>.<resource_name>
```

#### **Example:**
```bash
terraform destroy -target=aws_instance.my_instance
```
This removes only the `aws_instance.my_instance` resource.

---
## terraform provision blocks
```hcl
# Define the AWS provider
provider "aws" {
  region = "us-east-1"   
}

# Create an EC2 instance
resource "aws_instance" "my_ec2" {
  ami           = "ami-0ecb62995f68bb549" 
  instance_type = "t3.micro"  
  key_name = "nv" 



```
# 🚀 Terraform Provisioners Practical (EC2 + Local, File & Remote)

## 🎯 Objective

Create an EC2 instance using Terraform and use all three provisioners:

- 💻 **local-exec**
- 📁 **file**
- 🌐 **remote-exec**

---

# ✅ Prerequisites

## 🔑 Step 1: Create an AWS Key Pair

Navigate to:

```text
AWS Console → EC2 → Key Pairs → Create Key Pair
```

Example:

```text
Key Name : terraform-key
Type     : RSA
Format   : .pem
```

Download the private key:

```text
terraform-key.pem
```

📌 Place the `.pem` file inside your Terraform project directory.

---

## 🛡️ Step 2: Create a Security Group

Add the following inbound rules:

| Port | Protocol | Source | Purpose |
|------|----------|--------|---------|
| 22 | SSH | Your IP | SSH Access |
| 80 | HTTP | 0.0.0.0/0 | Web Access |

Example Security Group ID:

```text
sg-xxxxxxxx
```

---

## 📂 Step 3: Project Structure

```text
terraform/
│
├── main.tf
├── hello.txt
└── terraform-key.pem
```

---

## 📝 Step 4: Create hello.txt

Create a file named **hello.txt**.

Content:

```text
Hello from Terraform Provisioner
```

---

# 🖥️ Step 5: Create main.tf

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "web" {

  ami                    = "ami-xxxxxxxxxxxxxxxx"
  instance_type          = "t2.micro"
  key_name               = "terraform-key"

  vpc_security_group_ids = ["sg-xxxxxxxx"]

  tags = {
    Name = "Provisioner-Demo"
  }

  #################################
  # 💻 Local Provisioner
  #################################

  provisioner "local-exec" {
    command = "echo EC2 Created Successfully > output.txt"
  }

  #################################
  # 📁 File Provisioner
  #################################

  provisioner "file" {

    source      = "hello.txt"

    destination = "/home/ec2-user/hello.txt"
  }

  #################################
  # 🌐 Remote Provisioner
  #################################

  provisioner "remote-exec" {

    inline = [

      "echo 'Reading File'",

      "cat /home/ec2-user/hello.txt",

      "touch demo.txt",

      "echo 'Provisioner Completed' > demo.txt"

    ]

  }

  #################################
  # 🔐 SSH Connection
  #################################

  connection {

    type        = "ssh"

    user        = "ec2-user"

    private_key = file("terraform-key.pem")

    host = self.public_ip

  }

}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

---

# ⚙️ Step 6: Initialize Terraform

```bash
terraform init
```

---

# ✅ Step 7: Validate the Configuration

```bash
terraform validate
```

---

# 📋 Step 8: Review the Execution Plan

```bash
terraform plan
```

---

# 🚀 Step 9: Create the Infrastructure

```bash
terraform apply
```

Type:

```text
yes
```

---

# 🔄 What Happens During Execution?

## 🖥️ EC2 Instance Creation

✅ Terraform creates a new EC2 instance.

---

## 💻 Local Provisioner

Creates a local file:

```text
output.txt
```

Content:

```text
EC2 Created Successfully
```

---

## 📁 File Provisioner

Copies the local file:

```text
hello.txt
```

➡️ To the EC2 instance:

```text
/home/ec2-user/hello.txt
```

---

## 🌐 Remote Provisioner

Runs the following commands inside the EC2 instance:

```bash
cat /home/ec2-user/hello.txt

touch demo.txt

echo "Provisioner Completed" > demo.txt
```

Creates a file named:

```text
demo.txt
```

---

# 🔍 Step 10: Verify the Result

Connect to the EC2 instance:

```bash
ssh -i terraform-key.pem ec2-user@<PUBLIC-IP>
```

List all files:

```bash
ls
```

Expected output:

```text
hello.txt
demo.txt
```

---

## 📄 Verify hello.txt

```bash
cat hello.txt
```

Expected output:

```text
Hello from Terraform Provisioner
```

---

## 📄 Verify demo.txt

```bash
cat demo.txt
```

Expected output:

```text
Provisioner Completed
```

---

# 🎉 What You Learned

✅ Launch an EC2 instance using Terraform

✅ Execute commands on your local machine using **local-exec**

✅ Copy files from your local machine to EC2 using the **file** provisioner

✅ Execute commands inside EC2 using **remote-exec**

✅ Configure SSH access using a private key

✅ Display the EC2 Public IP using an output block

---

# 🧠 Quick Summary

| Provisioner | Runs Where? | Purpose |
|-------------|------------|----------|
| 💻 local-exec | Local Machine | Execute local commands |
| 📁 file | Local ➜ EC2 | Copy files to EC2 |
| 🌐 remote-exec | EC2 | Execute commands inside EC2 |
## EKS Cluster through Terraform
```hcl
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
      },
    ]
  })
}

resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSClusterPolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
  role       = aws_iam_role.cluster.name
}


data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}

resource "aws_eks_cluster" "cluster" {
  name = "cluster"
  access_config {
    authentication_mode = "API"
    }
  role_arn = aws_iam_role.cluster.arn
  vpc_config {
    subnet_ids = data.aws_subnets.default.ids
  }
  depends_on = [
    aws_iam_role_policy_attachment.cluster_AmazonEKSClusterPolicy,
  ]   
}
```
---
