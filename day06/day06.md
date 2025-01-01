# TerraWeek Day 6: Terraform Providers

## Task 1: Learn Terraform Providers



### What are Terraform Providers?
Terraform providers act as plugins that enable Terraform to interact with external systems. Each provider is responsible for managing API interactions and providing a way to manage resources of a specific service or infrastructure platform (e.g., AWS, Azure, GCP).

### Why are Providers Important?
- Enable platform-agnostic infrastructure as code.
- Manage a wide variety of resources from a single tool.
- Provide abstractions over complex API calls.

---

### Learning Steps:

#### 1. Understanding Providers:
- Providers must be declared in the configuration file before managing resources.
- Each provider offers supported resource types and data sources that align with the capabilities of the platform.

#### Example Configuration:
```hcl
# Example with AWS Provider
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "purvesh-bucket"
  acl    = "private"
}
```

#### 2. Setting Up a Provider:
- Initialize Terraform to download provider plugins:
  ```bash
  terraform init
  ```

- Specify provider-specific configurations like regions, credentials, or endpoints.

---

## Task 2: Provider Configuration and Authentication

### Objective:
Explore provider configuration and set up authentication for each provider.

---

### Steps:

####  AWS Provider Configuration and Authentication
- Install the AWS CLI and configure it with your credentials:
  ```bash
  aws configure
  ```
- Example Provider Configuration in Terraform:
  ```hcl
  provider "aws" {
    region     = "us-east-1"
    access_key = "<your_access_key>" 
    secret_key = "<your_secret_key>" 
  }
  ```
- Alternatively, use an IAM Role or environment variables:
  ```bash
  export AWS_ACCESS_KEY_ID=<your_access_key>
  export AWS_SECRET_ACCESS_KEY=<your_secret_key>
  ```


### Best Practices for Authentication:
- Use environment variables or credentials files to avoid hardcoding sensitive information.
- Follow the principle of least privilege by assigning minimal permissions required.
- Rotate keys regularly to enhance security.

---

## Task 3: Practice Using Providers

### Objective:
Gain hands-on experience using Terraform providers for AWS Cloud.

---

### Steps:

#### 1. Create a Terraform Configuration File
- Create a file named `main.tf` with the following content to configure the AWS provider and deploy a simple resource:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "example" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "PurveshVPC"
  }
}
```

#### 2. Authenticate with AWS
- Use the AWS CLI or environment variables for authentication:
  ```bash
  aws configure
  ```

#### 3. Deploy a Resource
Initialize, Plan and Apply Terraform to download required provider plugins:
  ```bash
  terraform init
  terraform plan
  terraform apply
  ```

#### 4. Update the Configuration
- Modify the resource configuration, for example, adding more tags:
  ```hcl
  tags = {
    Name    = "PurveshVPC"
    Project = "TerraWeek"
  }
  ```
- Apply the changes:
  ```bash
  terraform apply
  ```

#### 5. Destroy Resources
- Clean up by destroying the resources created:
  ```bash
  terraform destroy
  ```

---

End of Day 06
