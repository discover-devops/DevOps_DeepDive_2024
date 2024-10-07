### **Day 5: Continuous Monitoring & Infrastructure Provisioning**

#### **Hour 3: Introduction to Terraform**

---

#### **Concepts: Writing Secure Terraform Configurations for Infrastructure Provisioning**

**Terraform** is an open-source **Infrastructure as Code (IaC)** tool that allows you to define, provision, and manage infrastructure resources across various cloud providers (e.g., AWS, Azure, GCP) using a declarative configuration language. Terraform simplifies the automation of infrastructure deployment, reducing manual efforts and enabling repeatable and consistent infrastructure management.

---

**Key Terraform Concepts**:

1. **Providers**:
   - Terraform uses providers (e.g., AWS, Azure, GCP) to interact with cloud platforms and services. Each provider requires credentials and is configured to deploy infrastructure resources on a specific platform.

2. **Resources**:
   - Resources are the fundamental building blocks of Terraform configurations. They represent infrastructure objects such as virtual machines, networks, storage, and databases.

3. **State**:
   - Terraform maintains the state of the deployed infrastructure, keeping track of the resources it has created. The state allows Terraform to determine what changes need to be applied in subsequent runs.

4. **Security Best Practices**:
   - Use **least privilege** for cloud provider credentials.
   - Store sensitive information, such as access keys, using secure methods like **Terraform Vault** or environment variables.
   - Regularly review and apply security updates to Terraform configurations and modules.

---

**Why Use Terraform**:
- **Consistency**: Ensures that infrastructure is provisioned in a consistent and repeatable manner across environments.
- **Scalability**: Allows scaling resources automatically based on configuration changes.
- **Auditable Infrastructure**: Changes to infrastructure are version-controlled, allowing easy review and auditing of modifications.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Automating Virtual Machine Provisioning**:
   - A company uses **Terraform** to automate the provisioning of virtual machines across multiple cloud providers (AWS, Azure). The same configuration is used to ensure that the environments are consistent, and changes are applied in an auditable and repeatable manner.

2. **Real-World Use Case: Secure Infrastructure Deployment**:
   - A DevOps team uses Terraform to deploy a secure cloud infrastructure, ensuring that only specific IPs have access to certain resources (e.g., limiting SSH access) and that all configurations are encrypted and stored securely.

---

#### **Hands-on Lab: Write Terraform Configurations to Provision Virtual Machines**

In this lab, participants will learn to write secure Terraform configurations to provision virtual machines (VMs) in a cloud environment. They will use Terraform to define the infrastructure and securely manage sensitive data.

---

### **Lab 1: Writing Secure Terraform Configurations for Virtual Machines**

**Prerequisites**:
- An AWS account (or any cloud provider of your choice).
- **Terraform** installed on your local machine.

---

**Step 1: Install Terraform**

1. **Install Terraform** on your local machine:
   - On Linux/macOS:
     ```bash
     curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
     sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
     sudo apt-get update && sudo apt-get install terraform
     ```

2. **Verify Installation**:
   - After installation, verify that Terraform is installed:
     ```bash
     terraform --version
     ```

---

**Step 2: Write a Terraform Configuration to Provision a Virtual Machine**

1. **Create a Terraform Directory**:
   - Create a directory to store your Terraform configuration files:
     ```bash
     mkdir terraform-vm-setup && cd terraform-vm-setup
     ```

2. **Create the Main Terraform Configuration File** (`main.tf`):
   - Write a Terraform configuration file to provision a **virtual machine** on AWS:
     ```hcl
     provider "aws" {
       region = "us-west-2"
       access_key = "your-access-key"
       secret_key = "your-secret-key"
     }

     resource "aws_instance" "my_vm" {
       ami           = "ami-0abcdef1234567890"  # Replace with a valid AMI ID
       instance_type = "t2.micro"

       tags = {
         Name = "Terraform-VM"
       }
     }

     output "instance_ip" {
       value = aws_instance.my_vm.public_ip
     }
     ```

3. **Secure the Access Keys**:
   - **Never hardcode sensitive data** like access keys in your Terraform configurations. Instead, use environment variables to provide credentials securely:
     ```bash
     export AWS_ACCESS_KEY_ID="your-access-key"
     export AWS_SECRET_ACCESS_KEY="your-secret-key"
     ```

   - Modify the `provider` block to use environment variables:
     ```hcl
     provider "aws" {
       region = "us-west-2"
     }
     ```

---

**Step 3: Initialize and Apply the Terraform Configuration**

1. **Initialize the Terraform Working Directory**:
   - Initialize the Terraform directory to download provider plugins (e.g., AWS):
     ```bash
     terraform init
     ```

2. **Check the Terraform Plan**:
   - Before applying any changes, run `terraform plan` to review the resources that will be created:
     ```bash
     terraform plan
     ```

3. **Apply the Configuration**:
   - Apply the Terraform configuration to provision the virtual machine:
     ```bash
     terraform apply
     ```

4. **Verify the VM Creation**:
   - After the process completes, Terraform will output the **public IP address** of the newly created VM:
     ```bash
     Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

     Outputs:
     instance_ip = "XX.XX.XX.XX"
     ```

   - Use the public IP to connect to the instance via SSH:
     ```bash
     ssh -i your-key.pem ubuntu@XX.XX.XX.XX
     ```

---

**Step 4: Secure the Terraform State and Resources**

1. **State Management**:
   - Terraform tracks the infrastructure it manages through a **state file**. Make sure the state file is secure by using remote storage solutions (e.g., **S3 with encryption**).
   
   - Example of remote state storage with S3:
     ```hcl
     terraform {
       backend "s3" {
         bucket = "my-terraform-state"
         key    = "terraform/vm/terraform.tfstate"
         region = "us-west-2"
         encrypt = true
       }
     }
     ```

2. **Use Output Variables for Sensitive Information**:
   - When dealing with sensitive information (like passwords or database credentials), ensure output variables are **sensitive**:
     ```hcl
     output "instance_password" {
       value     = random_password.my_password.result
       sensitive = true
     }
     ```

---

**Step 5: Destroy Resources When Done**

1. **Destroy the Provisioned Resources**:
   - Once you no longer need the resources, destroy them using the following command:
     ```bash
     terraform destroy
     ```

---

### **Lab Wrap-Up**

By the end of this lab, participants will have:
- Written a secure Terraform configuration to provision a virtual machine.
- Applied the configuration to create infrastructure on AWS.
- Managed sensitive information and secured Terraform state.
- Destroyed the provisioned resources when no longer needed.

---

This lab provides hands-on experience with **Terraform** for infrastructure provisioning, focusing on writing secure configurations and managing cloud infrastructure. 
