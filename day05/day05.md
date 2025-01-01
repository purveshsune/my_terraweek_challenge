# TerraWeek Day 5

## Task 1: Understanding Modules in Terraform

### What are Modules in Terraform?
Modules in Terraform are containers for multiple resources that are used together. A module can consist of one or more `.tf` files in a directory and encapsulates reusable and logical infrastructure components.

### Why Do We Need Modules in Terraform?
Modules enable:
- **Reusability**: Write once and use across multiple configurations.
- **Organization**: Simplify complex infrastructure by grouping resources.
- **Consistency**: Ensure consistent configurations across environments.
- **Scalability**: Break down large configurations into manageable parts.

### Benefits of Using Modules in Terraform
- Simplified management of related resources.
- Encourages the Don't Repeat Yourself principle.
- Easier collaboration and maintenance.
- Version control for consistency across deployments.

---

## Task 2: Create a Module for AWS EC2 Instance

### Define the Module
1. **Create a Module Directory**: `modules/ec2`
2. **Create Files in the Module**:
   - `main.tf`:
     ```hcl
     variable "instance_name" {
       type        = string
       description = "Name of the EC2 instance"
     }

     variable "instance_type" {
       type        = string
       default     = "t2.micro"
       description = "EC2 instance type"
     }

     variable "ami" {
       type        = string
       description = "AMI ID for the EC2 instance"
     }

     resource "aws_instance" "example" {
       ami           = var.ami
       instance_type = var.instance_type

       tags = {
         Name = var.instance_name
       }
     }
     ```
   - `variables.tf`: Define variables as shown above.
   - `outputs.tf`:
     ```hcl
     output "instance_id" {
       value = aws_instance.example.id
     }

     output "instance_public_ip" {
       value = aws_instance.example.public_ip
     }
     ```

### Use the Module in the Root Configuration
Create a `main.tf` in the root directory:
```hcl
provider "aws" {
  region = "us-east-1"
}

module "ec2_instance" {
  source         = "./modules/ec2"
  instance_name  = "MyTerraformInstance"
  instance_type  = "t2.micro"
  ami            = "ami-0c55b159cbfafe1f0"
}

output "instance_id" {
  value = module.ec2_instance.instance_id
}

output "instance_public_ip" {
  value = module.ec2_instance.instance_public_ip
}
```

### Steps to Apply:
Initialize, Plan & Apply Terraform:
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```

---

## Task 3: Modular Composition and Module Versioning

### Modular Composition
- Combine multiple modules to build complex infrastructure.
- Example:
  ```hcl
  module "network" {
    source = "./modules/network"
  }

  module "application" {
    source          = "./modules/application"
    vpc_id          = module.network.vpc_id
    subnet_ids      = module.network.subnet_ids
  }
  ```

### Module Versioning
- For version control, use modules from a registry or git repository.
- Example:
  ```hcl
  module "example_module" {
    source  = "git::https://github.com/terraform-aws-modules/terraform-aws-ec2-instance.git?ref=v2.0.0"
  }
  ```

---

## Task 4: Locking Terraform Module Versions

### Why Lock Versions?
Locking ensures consistency and avoids unintended changes.

---

End of Day 5.
