# TerraWeek Day 7 - Advanced Terraform Topics

## Task 1: Workspaces, Remote Execution, and Collaboration

### Objective:
Gain proficiency in using workspaces, remote execution, and collaboration features in Terraform.

### Steps:

#### 1. Understand Terraform Workspaces:
- **What Are Workspaces?**
  Workspaces allow managing multiple environments (e.g., development, staging, production) in a single Terraform configuration.
- **How to Use Workspaces:**
  - Create a new workspace:
    ```bash
    terraform workspace new <workspace_name>
    ```
  - List all workspaces:
    ```bash
    terraform workspace list
    ```
  - Switch to a workspace:
    ```bash
    terraform workspace select <workspace_name>
    ```

#### 2. Remote Execution with Remote Backends:
- **Why Remote Backends?**
  - Enable centralized state management.
  - Facilitate collaboration among team members.

- **Setup Remote Backend with AWS S3:**
  ```hcl
  terraform {
    backend "s3" {
      bucket         = "day7-terraform-state-bucket"
      key            = "path/to/terraform.tfstate"
      region         = "us-east-1"
      dynamodb_table = "terraform-lock"
    }
  }
  ```
  - Initialize backend:
    ```bash
    terraform init
    ```

#### 3. Collaboration Tools:
- **HashiCorp Terraform Cloud:**
  - Centralized platform for state management, workspaces, and remote execution.
  - Steps to use:
    1. Create an account on Terraform Cloud.
    2. Set up a new workspace.
    3. Connect your Terraform Cloud workspace to your Git repository.

- **Terraform Enterprise:**
  - Enhanced collaboration and security features for larger teams.
  - Provides audit logging, SSO, and advanced role-based access.

---

## Task 2: Terraform Best Practices

### Objective:
Learn and implement best practices for organizing your Terraform code, version control, and CI/CD integration.

### Steps:

#### 1. Code Organization:
- Use **modular code** to make configurations reusable and manageable.
- Example Structure:
  ```
  terraform-project/
  ├── modules/
  │   ├── ec2/
  │   └── vpc/
  ├── environments/
  │   ├── dev/
  │   └── prod/
  ├── main.tf
  ├── variables.tf
  └── outputs.tf
  ```

#### 2. Version Control:
- **Git Best Practices:**
  - Use branching strategies like GitFlow or trunk-based development.
  - Commit frequently and use meaningful commit messages.
  - Example Commit Message:
    ```
    feat(vpc): Add VPC module with subnets and route tables
    ```

#### 3. CI/CD Integration:
- Use tools like **GitHub Actions**, **CircleCI**, or **Jenkins** for CI/CD pipelines.
- **Example Workflow:**
  - Run `terraform fmt` and `terraform validate` during pull requests.
  - Execute `terraform plan` to review infrastructure changes.
  - Apply changes after approval.
- Example GitHub Actions Configuration:
  ```yaml
  name: Terraform CI/CD

  on:
    pull_request:
      paths:
        - '**.tf'

  jobs:
    terraform:
      runs-on: ubuntu-latest

      steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1
        with:
          terraform_version: 1.5.0

      - name: Terraform Format Check
        run: terraform fmt -check

      - name: Terraform Validate
        run: terraform validate

      - name: Terraform Plan
        run: terraform plan
  ```

---

## Task 3: Exploring Additional Features



### Steps:

#### 1. Terraform Cloud/Enterprise:
- **Features:**
  - Collaboration on configurations.
  - Workflow automation (e.g., automatic apply).
  - Audit logging and RBAC.
- **Setup Steps:**
  1. Sign up for Terraform Cloud.
  2. Create a new workspace linked to your version control repository.
  3. Configure variables and secrets directly in the workspace.

#### 2. Terraform Registry:
- **What is Terraform Registry?**
  - A centralized repository for public and private modules and providers.
- **Using a Module from the Registry:**
  - Example:
    ```hcl
    module "vpc" {
      source  = "terraform-aws-modules/vpc/aws"
      version = "3.19.0"

      name    = "my-vpc"
      cidr    = "10.0.0.0/16"
      azs     = ["us-east-1a", "us-east-1b"]
      public_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
    }
    ```
  - Execute:
    ```bash
    terraform init
    terraform plan
    terraform apply
    ```

#### 3. Exploring Advanced Use Cases:
- Integrating with policy-as-code tools like HashiCorp Sentinel.
- Using Terraform CDK to write configurations in TypeScript or Python.
- Leveraging provider-specific advanced features like AWS CloudFormation compatibility.


---

# End of day07