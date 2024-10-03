### **Day 3: Configuration Management with Security Considerations**

#### **Hour 4: Secure Configuration Management with Puppet**

---

#### **Concepts: Role-Based Access Control (RBAC) and Secure Puppet Manifests**

In modern DevOps environments, especially when managing critical infrastructure, **secure configuration management** is crucial. With **Puppet**, we can implement role-based access control (RBAC) to enforce fine-grained permissions and secure manifests that ensure system configurations follow best security practices.

---

**Role-Based Access Control (RBAC)**:
- **RBAC** is a method of restricting access based on the roles of individual users within an organization. Puppet Enterprise supports **RBAC**, allowing administrators to define roles and assign specific permissions to users.
- **Permissions**: You can configure specific tasks that users or groups of users can perform, such as viewing node data, deploying code, or managing environments.
- **User Roles**: In Puppet Enterprise, roles can be defined for administrators, operators, developers, and auditors, each with specific permissions.

**Secure Puppet Manifests**:
- **Minimizing Privileges**: Ensuring that your Puppet code is designed to limit unnecessary privilege escalation.
- **Compliance**: Writing manifests that enforce compliance policies (e.g., ensuring only authorized users can SSH into a machine or managing firewall rules).
- **Hardening Configurations**: Applying security best practices, such as enforcing strong password policies, securing SSH access, or disabling unnecessary services.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Role-Based Access Control in Large Enterprises**:
   - A large financial institution uses **Puppet Enterprise** to manage the configurations of hundreds of servers. With RBAC, administrators ensure that only trusted personnel have access to sensitive configurations and infrastructure, enforcing strict security policies.
   
2. **Real-World Use Case: Hardening User Management**:
   - A company managing Linux servers for a large customer base uses Puppet to ensure only authorized users are allowed SSH access to production servers. They enforce a policy where SSH access requires key-based authentication, and root login is disabled.

---

#### **Hands-on Lab: Write a Puppet Manifest to Manage System Users with Security Best Practices**

In this lab, participants will write a **Puppet manifest** to manage system users, ensuring that security best practices are enforced, such as disabling password-based logins and ensuring strong password policies.

---

### **Lab 1: Managing System Users Securely with Puppet**

**Prerequisites**:
- Puppet installed on the Puppet Master server and Puppet agent on the target node.
- SSH access to the target node (e.g., an Ubuntu server).

---

**Step 1: Create a Puppet Manifest for Managing System Users**

1. On the **Puppet master node**, navigate to the manifests directory:
   ```bash
   cd /etc/puppetlabs/code/environments/production/manifests/
   ```

2. Create a new manifest file called `user_management.pp`:
   ```bash
   sudo touch user_management.pp
   ```

3. Edit the `user_management.pp` file with the following content to manage user accounts securely:
   ```puppet
   class user_management {

     # Ensure the 'devops' user exists and has secure password policies
     user { 'devops':
       ensure     => 'present',
       uid        => '1001',
       gid        => '1001',
       home       => '/home/devops',
       shell      => '/bin/bash',
       managehome => true,
     }

     # Set strong password policies for the devops user
     exec { 'set strong password for devops':
       command => "echo 'devops:$(openssl rand -base64 12)' | chpasswd",
       unless  => "grep devops /etc/shadow",
     }

     # Disable password-based SSH logins and root login
     file_line { 'disable password ssh login':
       path  => '/etc/ssh/sshd_config',
       line  => 'PasswordAuthentication no',
       match => '^PasswordAuthentication',
     }

     file_line { 'disable root ssh login':
       path  => '/etc/ssh/sshd_config',
       line  => 'PermitRootLogin no',
       match => '^PermitRootLogin',
     }

     # Restart SSH service to apply changes
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

**Explanation**:
- **user resource**: Ensures the `devops` user is created with a home directory and a specific shell.
- **exec resource**: Uses an `openssl` command to set a strong password for the `devops` user.
- **file_line resource**: Modifies the SSH configuration to disable password-based authentication and root login for security purposes.
- **service resource**: Restarts the SSH service after the configuration changes are applied.

---

**Step 2: Apply the Manifest on the Target Node**

1. Ensure the **Puppet agent** is running on the target node (e.g., `webserver`):
   ```bash
   sudo puppet agent --test
   ```

2. The `user_management.pp` manifest will be applied, creating the `devops` user, setting a strong password, and securing the SSH configuration.

---

**Step 3: Verify the Changes**

1. On the target node, verify that the `devops` user has been created:
   ```bash
   id devops
   ```

2. Check the `/etc/ssh/sshd_config` file to confirm that **password authentication** and **root login** are disabled:
   ```bash
   grep 'PasswordAuthentication' /etc/ssh/sshd_config
   grep 'PermitRootLogin' /etc/ssh/sshd_config
   ```

3. Attempt to SSH into the target node with password authentication (it should be disabled):
   ```bash
   ssh devops@192.168.1.10  # Replace with your server's IP address
   ```

---

### **Lab 2: Enforcing Strong Password Policies Using Puppet**

**Scenario**: In this lab, participants will extend their user management configuration to enforce a **strong password policy** using **PAM** (Pluggable Authentication Modules).

---

**Step 1: Update the Puppet Manifest for Strong Password Policies**

1. Modify the `user_management.pp` manifest to include a strong password policy:
   ```puppet
   class user_management {

     # Ensure the 'devops' user exists and has secure password policies
     user { 'devops':
       ensure     => 'present',
       uid        => '1001',
       gid        => '1001',
       home       => '/home/devops',
       shell      => '/bin/bash',
       managehome => true,
     }

     # Set strong password policies for the devops user
     exec { 'set strong password for devops':
       command => "echo 'devops:$(openssl rand -base64 12)' | chpasswd",
       unless  => "grep devops /etc/shadow",
     }

     # Disable password-based SSH logins and root login
     file_line { 'disable password ssh login':
       path  => '/etc/ssh/sshd_config',
       line  => 'PasswordAuthentication no',
       match => '^PasswordAuthentication',
     }

     file_line { 'disable root ssh login':
       path  => '/etc/ssh/sshd_config',
       line  => 'PermitRootLogin no',
       match => '^PermitRootLogin',
     }

     # Ensure PAM password policies are configured for strong passwords
     file_line { 'enforce password strength':
       path  => '/etc/pam.d/common-password',
       line  => 'password requisite pam_pwquality.so retry=3 minlen=12',
       match => '^password requisite pam_pwquality.so',
     }

     # Restart SSH and login services to apply changes
     service { 'ssh':
       ensure => 'running',
       enable => true,
       subscribe => File['/etc/ssh/sshd_config'],
     }

     service { 'login':
       ensure => 'running',
       enable => true,
       subscribe => File['/etc/pam.d/common-password'],
     }
   }

   node 'webserver' {
     include user_management
   }
   ```

---

**Step 2: Apply the Manifest Again**

1. Ensure the Puppet agent is running and applies the updated manifest on the target node:
   ```bash
   sudo puppet agent --test
   ```

2. Verify that the **PAM** configuration has been updated to enforce strong password policies by checking `/etc/pam.d/common-password`.

---

**Step 3: Test the Configuration**

1. Test by attempting to set a weak password for a user (it should fail):
   ```bash
   passwd devops
   ```

2. Set a strong password to confirm that the new policy is in effect.

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Written secure **Puppet manifests** to manage system users, ensuring they follow security best practices.
- Implemented **role-based access control** to limit SSH access and disable root logins.
- Enforced strong password policies using **PAM** via Puppet to harden the system’s security.

---

These labs give participants hands-on experience with writing secure Puppet manifests, ensuring they can enforce security policies and manage users securely in real-world environments.
