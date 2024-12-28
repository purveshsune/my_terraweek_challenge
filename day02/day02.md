# TerraWeek Day 2

## Task 1: Familiarize Yourself with HCL Syntax Used in Terraform

### HCL Syntax Basics
HCL (HashiCorp Configuration Language) is a declarative language used in Terraform configurations. 

- **Blocks**: The structural unit of Terraform code (e.g., `resource`, `provider`).
- **Parameters**: Define properties or options inside a block (e.g., `name`, `region`).
- **Arguments**: Assign values to parameters (e.g., `region = "us-west-1"`).

#### Example Block:
```hcl
provider "aws" {
  region = "us-west-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "purvesh-bucket"
  acl    = "private"
}
```

### Resources and Data Sources
- **Resources**: Represent components like instances, buckets, or databases that Terraform creates or manages.
- **Data Sources**: Fetch read-only information about existing infrastructure components for use in your configuration.

#### Example of a Data Source:
```hcl
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "purvesh"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }

  owners = ["00000000000"] 
}
```

---

## Task 2: Understand Variables, Data Types, and Expressions in HCL

### Creating a `variables.tf` File
Define a variable in a `variables.tf` file:
```hcl
variable "file_content" {
  type        = string
  default     = "This is a Terraform-created file."
  description = "Content for the local file created by purvesh"
}
```

### Using the Variable in `main.tf`
Use the variable in a resource definition:
```hcl
provider "local" {}

resource "local_file" "example" {
  filename = "purvesh.txt"
  content  = var.file_content
}
```

### Steps to Execute:
1. Create a `variables.tf` file and `main.tf` file in the same directory.
2. Initialize Terraform, plan and apply
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```

3. Verify the file creation locally.

---

## Task 3: Practice Writing Terraform Configurations Using HCL Syntax

### Adding `required_providers` to Your Configuration
Define required providers in `main.tf`:
```hcl
terraform {
  required_providers {
    docker = {
      source  = "hashicorp/docker"
      version = "~> 3.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "nginx" {
  name  = "purvesh-nginx-container"
  image = docker_image.nginx.latest
  ports {
    internal = 80
    external = 8080
  }
}
```

### Testing Your Configuration
Initialize Terraform to download required providers, check plan, apply configurations and then verify running containers:
   ```bash
   terraform init
   terraform plan
   terraform apply
   docker ps
   ```
