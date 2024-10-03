### **Day 3: Configuration Management with Security Considerations**

#### **Hour 3: Writing Ansible Roles and Playbooks**

---

#### **Concepts: Advanced Ansible Techniques**

In larger and more complex environments, Ansible **roles** are used to structure playbooks for reusability and better organization. Roles provide a way to automatically load related variables, tasks, handlers, and other files based on a known file structure.

---

**What Are Ansible Roles?**

Ansible **roles** are a way of breaking a playbook into multiple reusable components. Each role consists of tasks, handlers, files, templates, and variables, which can be defined separately and easily reused across different playbooks.

Key benefits of using roles:
- **Modularity**: Roles make playbooks more modular and reusable.
- **Separation of Concerns**: Each role focuses on a specific part of the infrastructure (e.g., database, web server).
- **Simplicity**: Playbooks become simpler by delegating tasks to roles.

---

**Ansible Role Directory Structure:**

Ansible roles follow a specific directory structure:
```
roles/
  └── role_name/
      ├── tasks/
      ├── handlers/
      ├── files/
      ├── templates/
      ├── vars/
      ├── defaults/
      └── meta/
```

- **tasks/**: Contains the main list of tasks to be executed by the role.
- **handlers/**: Contains handlers triggered by tasks.
- **files/**: Stores static files that can be copied to target machines.
- **templates/**: Stores templates (e.g., Jinja2 templates) for generating configuration files dynamically.
- **vars/**: Stores variables specific to the role.
- **defaults/**: Stores default variables for the role (can be overridden).
- **meta/**: Provides metadata about the role, like dependencies.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Web Server Role**:
   - In a DevOps setup, teams often deploy multiple instances of web servers (e.g., Apache, Nginx). Using an **Ansible role** for web server setup ensures the configuration is reusable across environments (e.g., staging, production), simplifying management.

2. **Real-World Use Case: Database Role**:
   - A company manages several different database servers (e.g., MySQL, PostgreSQL). Creating an **Ansible role** to manage database installations and configurations enables teams to deploy consistent database configurations across all servers without duplicating effort.

---

#### **Hands-on Lab: Develop Modular Playbooks Using Roles**

In this lab, participants will create and use Ansible roles to deploy a simple web server (Apache) with a modular playbook.

---

### **Lab 1: Creating an Ansible Role for Web Server Configuration**

**Prerequisites**:
- Ansible installed on the control node.
- SSH access to the remote server (target node).

---

**Step 1: Create a New Ansible Role for Apache**

1. In your Ansible project directory, create a new role using Ansible's built-in command:
   ```bash
   ansible-galaxy init apache-role
   ```

   This will create the necessary directory structure for the role under `roles/apache-role/`.

---

**Step 2: Define Tasks for the Role**

1. Navigate to the `roles/apache-role/tasks/` directory:
   ```bash
   cd roles/apache-role/tasks
   ```

2. Edit the `main.yml` file to add tasks for installing and configuring Apache:
   ```yaml
   ---
   - name: Install Apache
     apt:
       name: apache2
       state: present
     become: true

   - name: Start and enable Apache service
     service:
       name: apache2
       state: started
       enabled: true
     become: true
   ```

---

**Step 3: Create a Playbook to Use the Role**

1. Go back to the root of your project directory and create a new playbook called `site.yml`:
   ```bash
   touch site.yml
   ```

2. Add the following content to `site.yml` to call the **apache-role**:
   ```yaml
   ---
   - hosts: webservers
     become: true
     roles:
       - apache-role
   ```

---

**Step 4: Define the Inventory File**

1. Create an inventory file (`inventory.ini`) with the IP address of your target server:
   ```ini
   [webservers]
   192.168.1.10  # Replace with your server IP address
   ```

---

**Step 5: Run the Playbook with the Apache Role**

1. Run the playbook, which will invoke the Apache role and install the web server:
   ```bash
   ansible-playbook -i inventory.ini site.yml
   ```

2. Verify that Apache is installed and running by visiting the server’s IP address in a browser (`http://192.168.1.10`).

---

### **Lab 2: Creating an Ansible Role for MySQL Configuration**

**Scenario**: In this lab, participants will create an Ansible role for MySQL installation and configuration, demonstrating how to reuse roles for different services.

---

**Step 1: Create a New Role for MySQL**

1. Generate a new role for MySQL using Ansible Galaxy:
   ```bash
   ansible-galaxy init mysql-role
   ```

---

**Step 2: Define Tasks for MySQL Installation**

1. Navigate to `roles/mysql-role/tasks/` and edit the `main.yml` file:
   ```yaml
   ---
   - name: Install MySQL
     apt:
       name: mysql-server
       state: present
     become: true

   - name: Start and enable MySQL service
     service:
       name: mysql
       state: started
       enabled: true
     become: true
   ```

---

**Step 3: Update the Playbook to Include the MySQL Role**

1. Edit the `site.yml` playbook to include both the **apache-role** and **mysql-role**:
   ```yaml
   ---
   - hosts: webservers
     become: true
     roles:
       - apache-role
       - mysql-role
   ```

---

**Step 4: Run the Playbook to Install Apache and MySQL**

1. Run the playbook again to install both Apache and MySQL on the target server:
   ```bash
   ansible-playbook -i inventory.ini site.yml
   ```

2. Verify that both Apache and MySQL are installed and running on the server.

---

### **Lab 3: Using Variables in Roles**

**Scenario**: In this lab, participants will add variables to their roles to make them more dynamic and reusable.

---

**Step 1: Define Default Variables for Apache Role**

1. Navigate to `roles/apache-role/defaults/` and edit the `main.yml` file to add a default port for Apache:
   ```yaml
   ---
   apache_port: 80
   ```

---

**Step 2: Modify the Apache Task to Use the Variable**

1. Go back to `roles/apache-role/tasks/main.yml` and modify the `service` task to use the `apache_port` variable:
   ```yaml
   ---
   - name: Install Apache
     apt:
       name: apache2
       state: present
     become: true

   - name: Start and enable Apache service
     service:
       name: apache2
       state: started
       enabled: true
     become: true

   - name: Ensure Apache is listening on port {{ apache_port }}
     lineinfile:
       path: /etc/apache2/ports.conf
       regexp: '^Listen'
       line: "Listen {{ apache_port }}"
     become: true
   ```

---

**Step 3: Override the Variable in the Playbook**

1. In the `site.yml` playbook, override the default port by defining the variable:
   ```yaml
   ---
   - hosts: webservers
     become: true
     roles:
       - role: apache-role
         vars:
           apache_port: 8080
       - mysql-role
   ```

2. Run the playbook again to apply the changes:
   ```bash
   ansible-playbook -i inventory.ini site.yml
   ```

3. Verify that Apache is now listening on port **8080** instead of the default **80** by visiting `http://192.168.1.10:8080`.

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Created modular **Ansible roles** for installing and configuring web servers (Apache) and databases (MySQL).
- Integrated roles into a **playbook** to manage multiple services with a single playbook.
- Used **variables** in roles to make them dynamic and reusable across different environments.

---

These labs provide a hands-on approach to developing modular, scalable Ansible playbooks using roles.
