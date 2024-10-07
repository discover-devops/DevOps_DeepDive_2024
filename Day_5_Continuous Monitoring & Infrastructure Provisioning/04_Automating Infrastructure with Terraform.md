### **Day 5: Continuous Monitoring & Infrastructure Provisioning**

#### **Hour 4: Automating Infrastructure with Terraform**

---

#### **Concepts: Infrastructure Lifecycle Management with Terraform**

One of the core advantages of **Terraform** is its ability to manage the complete lifecycle of infrastructure resources—from creation to updates and eventual teardown. This process ensures that infrastructure is automatically provisioned, updated, and destroyed in a controlled, predictable, and repeatable way. **Terraform** enables teams to automate infrastructure management, reducing manual configuration errors and ensuring that resources are properly maintained.

---

**Key Concepts in Infrastructure Lifecycle Management**:

1. **Provisioning**:
   - Terraform allows you to declaratively define infrastructure resources, such as virtual machines, networks, and storage, and then **provision** them in a consistent manner.
   
2. **Updating Resources**:
   - Infrastructure resources can be modified by updating the **Terraform configuration**. Terraform will compare the current state with the desired state and apply only the necessary changes.
   
3. **Destruction**:
   - Terraform can safely and systematically destroy resources that are no longer needed, ensuring that stale infrastructure is cleaned up and costs are minimized.

4. **Idempotency**:
   - Terraform operations are **idempotent**, meaning that the same configuration can be applied repeatedly without unintended changes, making infrastructure management predictable.

---

**Why Automate Infrastructure Lifecycle Management**:
- **Consistency**: Automation ensures that infrastructure is provisioned and maintained consistently across environments (dev, staging, production).
- **Efficiency**: Automated infrastructure management saves time, reduces manual interventions, and avoids human errors.
- **Cost Control**: Automatically tearing down unused resources prevents cloud over-provisioning, keeping costs under control.
- **Agility**: Automation enables teams to rapidly deploy new environments or services in response to changing business needs.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Automated Resource Scaling**:
   - A company uses Terraform to automate the creation and scaling of infrastructure. By updating Terraform configurations, they can easily scale resources (e.g., add more virtual machines) to handle increased user traffic, ensuring availability and performance.

2. **Real-World Use Case: Tear Down of Development Environments**:
   - A development team uses Terraform to create temporary test environments. After testing is completed, Terraform is used to destroy these resources automatically, ensuring there are no lingering costs.

---

#### **Hands-on Lab: Automate the Creation and Management of Resources Using Terraform**

In this lab, participants will automate the creation, updating, and deletion of cloud resources using **Terraform**. They will write a configuration to deploy multiple resources and use Terraform to manage their lifecycle, including updates and destruction.

---

### **Lab 1: Automating Resource Lifecycle with Terraform**

**Prerequisites**:
- A cloud account (AWS, Azure, GCP, or another supported cloud provider).
- Terraform installed on your local machine (from the previous lab).

---

**Step 1: Write Terraform Configurations for Multiple Resources**

1. **Create a New Terraform Configuration Directory**:
   - Create a new directory for this Terraform project:
     ```bash
     mkdir terraform-automation && cd terraform-automation
     ```

2. **Define a Virtual Network and VM Configuration** (`main.tf`):
   - Write a Terraform configuration file to provision a **virtual network** and multiple **virtual machines** on AWS:
     ```hcl
     provider "aws" {
       region = "us-west-2"
     }

     resource "aws_vpc" "my_vpc" {
       cidr_block = "10.0.0.0/16"
       tags = {
         Name = "my-vpc"
       }
     }

     resource "aws_subnet" "my_subnet" {
       vpc_id     = aws_vpc.my_vpc.id
       cidr_block = "10.0.1.0/24"
       availability_zone = "us-west-2a"
     }

     resource "aws_instance" "my_vms" {
       count         = 2
       ami           = "ami-0abcdef1234567890"  # Replace with valid AMI ID
       instance_type = "t2.micro"
       subnet_id     = aws_subnet.my_subnet.id

       tags = {
         Name = "Terraform-VM-${count.index}"
       }
     }

     output "instance_ips" {
       value = aws_instance.my_vms[*].public_ip
     }
     ```

3. **Explanation**:
   - **VPC (Virtual Private Cloud)**: Creates a VPC with the specified CIDR block (`10.0.0.0/16`).
   - **Subnet**: Defines a subnet in the specified VPC for the instances to be deployed.
   - **EC2 Instances**: Two virtual machines are created using the `count` meta-argument, and their public IPs are outputted.

---

**Step 2: Initialize Terraform and Provision Resources**

1. **Initialize Terraform**:
   - Initialize the working directory to download the necessary provider plugins:
     ```bash
     terraform init
     ```

2. **Plan the Infrastructure**:
   - Before creating resources, use `terraform plan` to preview the changes Terraform will make:
     ```bash
     terraform plan
     ```

3. **Apply the Configuration**:
   - Apply the Terraform configuration to provision the network and virtual machines:
     ```bash
     terraform apply
     ```

4. **Verify Resource Creation**:
   - After the process completes, Terraform will output the **public IP addresses** of the two virtual machines. You can use these IPs to SSH into the instances.

---

**Step 3: Update the Terraform Configuration to Modify Resources**

1. **Add a Security Group to the Configuration**:
   - Modify the Terraform configuration to add a **security group** that allows SSH access:
     ```hcl
     resource "aws_security_group" "allow_ssh" {
       vpc_id = aws_vpc.my_vpc.id

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
         Name = "allow_ssh"
       }
     }

     resource "aws_instance" "my_vms" {
       count         = 2
       ami           = "ami-0abcdef1234567890"
       instance_type = "t2.micro"
       subnet_id     = aws_subnet.my_subnet.id
       security_groups = [aws_security_group.allow_ssh.name]

       tags = {
         Name = "Terraform-VM-${count.index}"
       }
     }
     ```

2. **Apply the Updated Configuration**:
   - Run `terraform apply` again to update the existing infrastructure and add the security group:
     ```bash
     terraform apply
     ```

   - Terraform will automatically detect the changes and update the existing VMs to include the new security group, allowing SSH access.

3. **Verify the Changes**:
   - Check that the security group has been applied by connecting to the VM via SSH using the public IP:
     ```bash
     ssh -i your-key.pem ubuntu@<instance_ip>
     ```

---

**Step 4: Destroy the Infrastructure When Done**

1. **Clean Up Resources**:
   - When the infrastructure is no longer needed, destroy the resources to avoid incurring unnecessary costs:
     ```bash
     terraform destroy
     ```

2. **Verify Destruction**:
   - Confirm that the resources have been successfully destroyed and there are no lingering resources in the cloud provider’s management console.

---

### **Lab Wrap-Up**

By the end of this lab, participants will have:
- Written a Terraform configuration to provision a virtual network and multiple VMs.
- Updated the configuration to modify infrastructure (e.g., adding security groups).
- Managed the complete lifecycle of resources, including the automated destruction of resources when no longer needed.

---

This lab provides practical experience in automating infrastructure lifecycle management using Terraform, including provisioning, updating, and destroying resources. 
