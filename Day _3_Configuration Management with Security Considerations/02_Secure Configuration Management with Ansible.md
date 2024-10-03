### **Day 3: Configuration Management with Security Considerations**

#### **Hour 2: Secure Configuration Management with Ansible**

---

#### **Concepts: Secure Playbooks and Ansible Vault for Secrets Management**

When managing infrastructure through configuration management tools like Ansible, security is a key consideration. Securing your configurations and sensitive information (like passwords, API keys, or credentials) is vital to prevent security breaches.

---

**Secure Playbooks in Ansible:**

- **Principle of Least Privilege**: Only provide the necessary privileges to perform tasks. Avoid running tasks as the root user unless absolutely required.
- **Idempotency**: Ensure that running the playbook multiple times will result in the same state, minimizing the risk of inconsistent configurations.
- **Service and Package Hardening**: When configuring services like web servers, ensure that unnecessary services are disabled, and only required ports are open.

---

**Ansible Vault for Secrets Management:**

**Ansible Vault** is a feature that allows you to encrypt sensitive data, such as passwords or secret keys, within Ansible playbooks or variables. This prevents plain-text secrets from being exposed in the code or configuration files.

- **Creating Encrypted Files**: Ansible Vault allows you to create encrypted files that can store sensitive information.
  - Command:
    ```bash
    ansible-vault create secret.yml
    ```

- **Editing Encrypted Files**: To modify an encrypted file, you can use the following command:
  ```bash
  ansible-vault edit secret.yml
  ```

- **Encrypting Existing Files**: If you already have a file with sensitive data, you can encrypt it:
  ```bash
  ansible-vault encrypt existing-file.yml
  ```

- **Decrypting Files**: To view the decrypted contents temporarily:
  ```bash
  ansible-vault decrypt secret.yml
  ```

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Secure Web Server Configuration with Ansible**:
   - A web hosting company wants to automate the deployment of their Apache web servers. However, since some configurations require storing sensitive information like database credentials, they use **Ansible Vault** to securely manage this data while ensuring their servers are hardened for security.

2. **Real-World Use Case: Managing API Keys for Cloud Providers**:
   - A DevOps team uses **Ansible Vault** to securely store API keys for AWS and other cloud providers. These secrets are encrypted, ensuring they are not exposed in playbooks or configuration files.

---

#### **Hands-on Lab: Write a Secure Playbook to Configure a Web Server with Security Hardening**

In this lab, participants will create a secure playbook using **Ansible Vault** to encrypt sensitive data (e.g., passwords) and configure a web server (Apache) with basic security hardening practices.

---

### **Lab 1: Writing a Secure Ansible Playbook with Ansible Vault**

**Prerequisites**:
- Ansible installed on the control node.
- SSH access to a remote Ubuntu server (target node).
- Ansible Vault set up for managing secrets.

---

**Step 1: Create a Playbook for Apache Web Server**

1. Create a new directory for the lab:
   ```bash
   mkdir secure-ansible-lab && cd secure-ansible-lab
   ```

2. Create a new playbook file `secure-apache.yml`:
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

       - name: Ensure Apache is started
         service:
           name: apache2
           state: started
           enabled: true

       - name: Disable unnecessary modules
         command: a2dismod status

       - name: Set up firewall to allow only HTTP and HTTPS
         ufw:
           rule: allow
           port: "{{ item }}"
           proto: tcp
         loop:
           - '80'
           - '443'

       - name: Create a secure directory for website
         file:
           path: /var/www/secure-site
           state: directory
           owner: www-data
           group: www-data
           mode: '0755'

       - name: Set basic authentication for Apache
         apache2_module:
           name: auth_basic
           state: present
   ```

---

**Step 2: Create an Ansible Vault-Encrypted File for Secrets**

1. Create an encrypted file to store sensitive information (e.g., database password, admin credentials):
   ```bash
   ansible-vault create secrets.yml
   ```

2. Inside the encrypted file, add the following content:
   ```yaml
   ---
   admin_password: "SuperSecretPassword"
   db_password: "DBSuperSecret"
   ```

   This file will store sensitive information securely.

---

**Step 3: Create an Inventory File**

1. Create an `inventory.ini` file that defines the target web server:
   ```ini
   [webservers]
   192.168.1.20  # Replace with your server's IP address
   ```

---

**Step 4: Run the Playbook**

1. Run the playbook, passing the Ansible Vault password to decrypt the `secrets.yml` file:
   ```bash
   ansible-playbook -i inventory.ini secure-apache.yml --ask-vault-pass
   ```

2. Verify the following:
   - **Apache is installed and running**.
   - Unnecessary modules (like `status`) are disabled.
   - Only **HTTP** and **HTTPS** ports are allowed through the firewall.
   - Basic security hardening measures are implemented for the web server.

---

### **Lab 2: Using Ansible Vault to Secure MySQL Credentials**

In this lab, participants will configure MySQL on a remote server using **Ansible Vault** to store the database password securely.

---

**Step 1: Write the Ansible Playbook for MySQL**

1. Create a new file `secure-mysql.yml`:
   ```yaml
   ---
   - hosts: databases
     become: true
     vars_files:
       - db-secrets.yml
     tasks:
       - name: Install MySQL
         apt:
           name: mysql-server
           state: present

       - name: Ensure MySQL is running
         service:
           name: mysql
           state: started
           enabled: true

       - name: Set root password securely
         mysql_user:
           name: root
           password: "{{ mysql_root_password }}"
           host_all: yes
           priv: '*.*:ALL'

       - name: Harden MySQL by removing test databases and anonymous users
         mysql_db:
           name: test
           state: absent
   ```

---

**Step 2: Encrypt the Database Password Using Ansible Vault**

1. Create a new encrypted file `db-secrets.yml`:
   ```bash
   ansible-vault create db-secrets.yml
   ```

2. Inside the file, store the MySQL root password:
   ```yaml
   ---
   mysql_root_password: "SuperSecureDBPassword"
   ```

---

**Step 3: Create an Inventory File**

1. Create an inventory file `db-inventory.ini`:
   ```ini
   [databases]
   192.168.1.30  # Replace with your database server's IP address
   ```

---

**Step 4: Run the Playbook**

1. Run the playbook, using the Ansible Vault password to decrypt the `db-secrets.yml` file:
   ```bash
   ansible-playbook -i db-inventory.ini secure-mysql.yml --ask-vault-pass
   ```

2. Verify that:
   - MySQL is installed and running.
   - The root password is securely set using **Ansible Vault**.
   - Test databases and anonymous users have been removed for security hardening.

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Written a secure Ansible playbook to configure a web server with security hardening.
- Used **Ansible Vault** to encrypt and manage sensitive data like passwords.
- Hardened both **Apache** and **MySQL** configurations to improve security.

---

These labs provide practical experience in securing infrastructure using Ansible and Ansible Vault, ensuring participants learn how to protect sensitive data and enforce security best practices. 
