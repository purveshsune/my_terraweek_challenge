# TerraWeek Day 3

## Task 1: Create a Terraform Configuration File

### Defining an AWS EC2 Instance
Create a `main.tf` file to define an AWS EC2 instance:
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0" # Amazon Linux 2
  instance_type = "t2.micro"

  tags = {
    Name = "purvesh-ec2"
  }
}
```

### Steps:
Initialize, Plan & Apply Terraform:
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```

Verify the EC2 instance is created in the AWS Console.

---

## Task 2: Check State Files and Validate Configuration

### Checking State Files
- The Terraform state file (`terraform.tfstate`) contains information about the resources managed by Terraform.
- Before running commands, ensure no inconsistencies by inspecting the state file:
   ```bash
   cat terraform.tfstate
   ```

### Validating Configuration
Run the `validate` command to ensure the configuration is correct:
```bash
terraform validate
```

#### Expected Output:
- If the configuration is valid:
  ```
  Success! The configuration is valid.
  ```
- If there are errors, Terraform will display details to fix.

---

## Task 3: Add a Provisioner to Configure the Resource

### Adding a Provisioner
Modify the EC2 instance configuration to include a `provisioner`:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "purvesh-ec2"
  }

  provisioner "remote-exec" {
    inline = [
      "sudo yum update -y",
      "echo 'Hello from Terraform!' > /home/ec2-user/hello.txt"
    ]
  }
}
```

### Applying Changes and Destroying Resources
1. Apply the changes:
   ```bash
   terraform apply
   ```
2. Verify the provisioner executed by connecting to the instance and checking `/home/ec2-user/hello.txt`.
3. Destroy the resources:
   ```bash
   terraform destroy
   ```

---

## Task 4: Add Lifecycle Management

### Adding Lifecycle Rules
Enhance the EC2 configuration with `lifecycle` management:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "purvesh-ec2"
  }

  lifecycle {
    prevent_destroy = true
  }
}
```

### Steps:
1. Apply the changes:
   ```bash
   terraform apply
   ```
2. Attempt to destroy:
   ```bash
   terraform destroy
   ```
   - Expected Output: The resource cannot be destroyed due to the `prevent_destroy` setting.
3. To allow destruction, remove `prevent_destroy` or adjust the lifecycle configuration and re-apply:
   ```bash
   terraform apply
   ```

---

#### END of day 3.