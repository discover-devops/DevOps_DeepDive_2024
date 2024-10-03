### **Day 3: Configuration Management with Security Considerations**

#### **Hour 1: Introduction to Configuration Management**

---

#### **Concepts: Overview of Infrastructure as Code (IaC)**

**Infrastructure as Code (IaC)** is the practice of managing and provisioning computing infrastructure using machine-readable configuration files, rather than through manual processes. This practice is crucial in modern DevOps as it ensures:

- **Consistency**: The infrastructure setup is repeatable, reducing human error.
- **Versioning**: Infrastructure configurations can be version-controlled like code, allowing rollback if needed.
- **Automation**: Automating the deployment of infrastructure reduces manual intervention and improves efficiency.

---

#### **Comparison of Puppet and Ansible**

Both **Puppet** and **Ansible** are popular tools used for configuration management, but they differ in key areas.

| Feature                 | Puppet                                     | Ansible                                    |
|-------------------------|--------------------------------------------|--------------------------------------------|
| **Architecture**         | Client-Server model (requires agents)      | Agentless (SSH-based)                      |
| **Language**             | Domain-Specific Language (DSL)             | YAML (simple, human-readable syntax)       |
| **Learning Curve**       | Steeper due to its DSL                     | Easier for beginners (uses YAML)           |
| **Configuration Model**  | Declarative                                | Mostly declarative, but allows procedural  |
| **Community Support**    | Large, enterprise-focused                  | Large, especially popular in DevOps        |
| **Use Cases**            | Complex, large-scale infrastructures       | Simpler setups, cloud environments         |

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Consistent Configuration with Puppet**:
   - A large financial institution uses Puppet to ensure that all their hundreds of servers are configured in exactly the same way. They rely on Puppet's client-server model to distribute configurations to every machine in their environment, guaranteeing consistency across both development and production servers.

2. **Real-World Use Case: Ansible for Cloud Deployments**:
   - A tech startup uses Ansible to deploy and manage infrastructure on AWS. Since Ansible is agentless and uses SSH, it’s easy to integrate with cloud providers. The YAML-based playbooks make it simple for new team members to understand and contribute to automation tasks.

---

#### **Hands-on Lab: Explore the Syntax of Ansible and Puppet**

In this lab, participants will explore the syntax and basic configurations in both **Ansible** and **Puppet** to understand their differences and practical applications.

---

### **Lab 1: Ansible - Writing a Basic Playbook**

An **Ansible playbook** defines a set of tasks to be executed on remote servers. This lab will guide participants through writing a simple playbook that installs **Apache** on a remote server.

**Prerequisites**:
- Ansible installed on your local machine (or the control node).
- SSH access to the target node (e.g., an Ubuntu server).

---

**Step 1: Install Ansible (If not already installed)**

- On a **Debian/Ubuntu** machine:
  ```bash
  sudo apt update
  sudo apt install ansible
  ```

- On a **CentOS/RHEL** machine:
  ```bash
  sudo yum install epel-release
  sudo yum install ansible
  ```

---

**Step 2: Write the Ansible Playbook**

1. Create a new directory for your Ansible project:
   ```bash
   mkdir ansible-lab && cd ansible-lab
   ```

2. Inside this directory, create a file named `apache-playbook.yml` with the following content:
   ```yaml
   ---
   - hosts: webservers
     become: true
     tasks:
       - name: Install Apache
         apt:
           name: apache2
           state: present
         when: ansible_os_family == 'Debian'

       - name: Start Apache service
         service:
           name: apache2
           state: started
           enabled: true
   ```

3. Explanation of the playbook:
   - **hosts**: Defines the target group (in this case, `webservers`).
   - **become**: Grants sudo privileges to the tasks.
   - **tasks**: An ordered list of operations, like installing packages or starting services.
   - **apt**: A module that installs packages (here it’s used to install Apache on Debian systems).
   - **service**: A module to start and enable the Apache service.

---

**Step 3: Define the Inventory File**

1. Create an inventory file (`inventory.ini`) to define the target servers:
   ```ini
   [webservers]
   192.168.1.10  # Replace with the IP address of your server
   ```

2. Ensure you have SSH access to the remote server by testing the connection:
   ```bash
   ansible -i inventory.ini all -m ping
   ```

---

**Step 4: Run the Playbook**

1. Run the playbook to install and configure Apache:
   ```bash
   ansible-playbook -i inventory.ini apache-playbook.yml
   ```

2. Verify that Apache has been installed on the remote server by visiting its IP address in a browser (`http://192.168.1.10`).

---

### **Lab 2: Puppet - Writing a Basic Manifest**

A **Puppet manifest** describes how systems should be configured. In this lab, participants will write a Puppet manifest that installs **Nginx** and ensures that the service is running.

**Prerequisites**:
- Puppet installed on the server (puppet master node).
- Puppet agent installed on the target nodes.

---

**Step 1: Install Puppet (If not already installed)**

- On the **Puppet master node** (Debian/Ubuntu):
  ```bash
  sudo apt update
  sudo apt install puppetserver
  ```

- On the **agent node**:
  ```bash
  sudo apt update
  sudo apt install puppet-agent
  ```

---

**Step 2: Write the Puppet Manifest**

1. On the Puppet master node, navigate to the **Puppet manifests directory**:
   ```bash
   cd /etc/puppetlabs/code/environments/production/manifests/
   ```

2. Create a new manifest file `nginx.pp` with the following content:
   ```puppet
   class nginx {
     package { 'nginx':
       ensure => installed,
     }

     service { 'nginx':
       ensure => running,
       enable => true,
     }
   }

   node 'webserver' {
     include nginx
   }
   ```

3. Explanation of the manifest:
   - **class**: Defines a reusable block of configuration.
   - **package**: Manages the installation of the Nginx package.
   - **service**: Ensures that the Nginx service is running and enabled at startup.
   - **node**: Specifies which node(s) the configuration should apply to.

---

**Step 3: Apply the Puppet Manifest**

1. On the agent node, ensure the Puppet agent is running:
   ```bash
   sudo puppet agent --test
   ```

2. The agent will apply the `nginx.pp` manifest and install Nginx on the target server.

3. Verify that Nginx is running by accessing the server’s IP in a browser (`http://192.168.1.20`).

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Written and executed an **Ansible playbook** to install and configure Apache on a remote server.
- Written a **Puppet manifest** to install and manage the Nginx service.
- Understood the differences in syntax and workflow between **Ansible** and **Puppet**.

---

These labs provide practical exposure to both Ansible and Puppet, helping participants understand the basics of configuration management. 
