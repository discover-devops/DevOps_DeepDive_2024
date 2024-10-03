### **Day 3: Configuration Management with Security Considerations**

#### **Hour 6: Q&A and Recap of Day 3**

---

**Recap**: During this hour, I will review the key concepts and labs covered throughout **Day 3**, including:

1. **Introduction to Configuration Management**: The basics of Infrastructure as Code (IaC) and a comparison of Ansible and Puppet.
2. **Secure Configuration Management with Ansible**: Writing secure playbooks, managing sensitive information using Ansible Vault, and configuring a web server with security hardening.
3. **Writing Ansible Roles**: Developing modular playbooks using Ansible roles for reusable and maintainable code.
4. **Secure Configuration Management with Puppet**: Implementing role-based access control (RBAC) and writing secure Puppet manifests to manage users and enforce best practices.
5. **Ansible vs. Puppet Discussion**: Comparative analysis of the two tools and real-world applications, exploring strengths, weaknesses, and use cases for each.

---

### **Assignments**

#### **Assignment 1: Secure Configuration Management with Ansible**

**Scenario**:  
Your organization is tasked with deploying a web application across multiple servers. To ensure the deployment is secure, you must use **Ansible** to configure the web servers while following security best practices. You’ll be required to manage secrets using **Ansible Vault** and implement basic security hardening for your application.

---

**Tasks**:

1. **Install Apache**: Write an Ansible playbook to install Apache on all web servers.
2. **Secure SSH Configuration**:
   - Disable password authentication for SSH.
   - Disable root login over SSH.
3. **Manage Secrets with Ansible Vault**:
   - Create an encrypted file using Ansible Vault to store sensitive information (e.g., MySQL root password).
   - Use Ansible Vault to securely manage and retrieve the password in your playbook.
4. **Apply Firewall Rules**: Ensure that only HTTP (port 80) and HTTPS (port 443) are allowed through the firewall.
5. **Run the Playbook**: Deploy your solution on a set of web servers.

**Deliverables**:
- The Ansible playbook that performs all tasks.
- The Ansible Vault file (encrypted) containing the sensitive information.

---

**Solution**:

1. **Ansible Playbook** (`secure-web-server.yml`):
   ```yaml
   ---
   - hosts: webservers
     become: true
     vars_files:
       - secrets.yml

     tasks:
       - name: Install Apache
         apt:
           name: apache2
           state: present

       - name: Disable password-based SSH logins
         lineinfile:
           path: /etc/ssh/sshd_config
           regexp: '^PasswordAuthentication'
           line: 'PasswordAuthentication no'

       - name: Disable root login over SSH
         lineinfile:
           path: /etc/ssh/sshd_config
           regexp: '^PermitRootLogin'
           line: 'PermitRootLogin no'

       - name: Restart SSH service to apply changes
         service:
           name: ssh
           state: restarted

       - name: Allow HTTP and HTTPS through the firewall
         ufw:
           rule: allow
           port: "{{ item }}"
           proto: tcp
         loop:
           - '80'
           - '443'

       - name: Set MySQL root password securely
         mysql_user:
           name: root
           password: "{{ mysql_root_password }}"
           host_all: yes
   ```

2. **Ansible Vault File** (`secrets.yml`):
   ```yaml
   ---
   mysql_root_password: "SecureRootPassword"
   ```

3. **To run the playbook**:
   ```bash
   ansible-playbook -i inventory.ini secure-web-server.yml --ask-vault-pass
   ```

---

#### **Assignment 2: Write a Puppet Manifest to Manage System Users Securely**

**Scenario**:  
Your organization requires a centralized method to manage system users across all Linux servers. As part of the security policy, only authorized users should be able to log in to production servers. You are tasked with writing a **Puppet manifest** that ensures the following:
- A **devops** user is created with a strong password.
- Only the **devops** user is allowed SSH access to the server.
- Password-based SSH login is disabled, and SSH keys are enforced.

---

**Tasks**:

1. **Create the `devops` user**: Write a Puppet manifest to create the `devops` user with a strong password and ensure that the home directory is properly configured.
2. **Configure SSH Access**:
   - Only the `devops` user should be able to access the server via SSH.
   - Password authentication for SSH should be disabled, and SSH key authentication should be enforced.
3. **Ensure the configuration is applied**: Apply the manifest across your servers and ensure the settings are properly enforced.

**Deliverables**:
- The **Puppet manifest** that manages the `devops` user and secures SSH access.

---

**Solution**:

1. **Puppet Manifest** (`user_management.pp`):
   ```puppet
   class user_management {

     # Create devops user with secure home directory and shell
     user { 'devops':
       ensure     => 'present',
       uid        => '1001',
       gid        => '1001',
       home       => '/home/devops',
       shell      => '/bin/bash',
       managehome => true,
     }

     # Set a strong password for devops
     exec { 'set strong password for devops':
       command => "echo 'devops:$(openssl rand -base64 12)' | chpasswd",
       unless  => "grep devops /etc/shadow",
     }

     # Disable password-based SSH logins
     file_line { 'disable password ssh login':
       path  => '/etc/ssh/sshd_config',
       line  => 'PasswordAuthentication no',
       match => '^PasswordAuthentication',
     }

     # Enforce SSH key-based authentication
     file_line { 'enable ssh key authentication':
       path  => '/etc/ssh/sshd_config',
       line  => 'PubkeyAuthentication yes',
       match => '^#PubkeyAuthentication',
     }

     # Disable root SSH login
     file_line { 'disable root ssh login':
       path  => '/etc/ssh/sshd_config',
       line  => 'PermitRootLogin no',
       match => '^PermitRootLogin',
     }

     # Restart SSH service
     service { 'ssh':
       ensure => 'running',
       enable => true,
       subscribe => File['/etc/ssh/sshd_config'],
     }
   }

   node 'webserver' {
     include user_management
   }
   ```

2. **Applying the Manifest**:
   - Ensure that Puppet agent is running on the target nodes and apply the manifest:
     ```bash
     sudo puppet agent --test
     ```

3. **Verification**:
   - SSH into the server using the `devops` user with SSH keys.
   - Ensure password-based login and root SSH access are disabled.

---

### **Lab Wrap-Up**

By the end of these assignments, participants will:
- Have practical experience in writing secure Ansible playbooks and Puppet manifests.
- Be able to manage system users and SSH configurations securely across their environments.
- Use Ansible Vault and Puppet manifests to manage sensitive information and secure system configurations.

These assignments ensure participants gain hands-on experience in securing infrastructure configurations using both Ansible and Puppet.
