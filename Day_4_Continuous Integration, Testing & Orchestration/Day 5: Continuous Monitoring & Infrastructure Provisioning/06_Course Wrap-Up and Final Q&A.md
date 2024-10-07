### **Day 5: Continuous Monitoring & Infrastructure Provisioning**

#### **Hour 6: Course Wrap-Up and Final Q&A**

---

**Recap**: In the final session, we will review the key topics covered throughout the course and address any remaining questions from participants. The goal of this session is to ensure that participants leave with a solid understanding of DevOps, DevSecOps, continuous integration, monitoring, infrastructure provisioning, and security best practices.

---

### **Course Summary**:

1. **Day 1: Agile Software Engineering & DevOps Fundamentals**
   - Covered the principles of Agile and how DevOps aligns with continuous development, integration, and delivery.
   - Hands-on activities included user story creation, effort estimation, and mapping the DevOps lifecycle onto sample projects.

2. **Day 2: Development Environment & Version Control**
   - Focused on setting up development environments, version control using Git, secure Git practices, and Docker for containerization.
   - Participants learned secure branching and merging strategies and how to build and run containers.

3. **Day 3: Configuration Management with Security Considerations**
   - Covered the use of **Ansible** and **Puppet** for secure configuration management, including writing roles and playbooks, and implementing secure RBAC practices.
   - Hands-on labs focused on writing secure playbooks and Puppet manifests.

4. **Day 4: Continuous Integration, Testing & Orchestration**
   - Explored CI principles using Jenkins, automating testing with Selenium, and container orchestration with Kubernetes.
   - Participants deployed applications to Kubernetes and integrated security testing in CI pipelines.

5. **Day 5: Continuous Monitoring & Infrastructure Provisioning**
   - Discussed continuous monitoring using the ELK Stack, infrastructure automation using Terraform, and security best practices in DevSecOps.
   - Hands-on labs covered monitoring infrastructure health using Kibana, automating infrastructure provisioning with Terraform, and implementing DevSecOps best practices.

---

### **2 Assignments for Day 5**:

#### **Assignment 1: Automate Infrastructure Monitoring and Alerting with ELK and Terraform**

**Scenario**:  
Your company has deployed multiple services on AWS and requires an automated solution for monitoring system performance and sending alerts when critical thresholds are met. You will integrate **Terraform** to automate the deployment of the ELK Stack and set up **Kibana dashboards** to monitor infrastructure health. Additionally, you will create alerts for critical events like high CPU usage or disk space exhaustion.

---

**Tasks**:

1. **Provision Infrastructure with Terraform**:
   - Write a Terraform configuration to provision virtual machines, including security groups and the ELK Stack.
   - Ensure the infrastructure is properly configured for log collection and monitoring.

2. **Set Up Continuous Monitoring**:
   - Configure the ELK Stack to collect system logs (e.g., `/var/log/syslog`).
   - Create a **Kibana dashboard** that visualizes key metrics like CPU, memory, and disk usage.

3. **Configure Alerts for Critical Events**:
   - Set up alerts in Kibana for high CPU usage or disk space reaching a critical threshold (e.g., 90%).
   - Configure the alerts to notify the DevOps team via email or Slack.

**Deliverables**:
- Terraform configuration files for provisioning the ELK Stack and VMs.
- Kibana dashboard screenshots displaying system performance metrics.
- A description of the alerts and notification configurations.

---

**Solution**:

1. **Terraform Configuration**:
   - A complete `main.tf` file provisioning virtual machines and the ELK Stack.
   - Outputs that display the public IPs of the provisioned instances.

2. **Kibana Dashboard**:
   - Dashboards showing visualizations of CPU usage, memory, disk space, and system performance.

3. **Alert Configuration**:
   - Email or Slack alerts configured to notify the DevOps team when critical thresholds are breached.

---

#### **Assignment 2: Implement DevSecOps Practices in a CI/CD Pipeline**

**Scenario**:  
You have been asked to ensure that security is integrated into your company's CI/CD pipeline. Using **Jenkins**, you will implement security testing (e.g., static and dynamic application security testing), secure configuration management (e.g., using Ansible Vault), and continuous monitoring for potential vulnerabilities in the pipeline.

---

**Tasks**:

1. **Set Up Security Testing**:
   - Integrate a **SAST tool** (e.g., SonarQube) into the Jenkins pipeline to scan code for security vulnerabilities.
   - Set up **DAST tools** like OWASP ZAP to perform dynamic security testing on deployed applications.

2. **Secure Configuration Management**:
   - Use **Ansible Vault** to securely store sensitive information such as API keys and credentials.
   - Write Ansible playbooks to configure a secure environment, ensuring that access to critical resources is restricted.

3. **Continuous Monitoring and Alerts**:
   - Monitor Jenkins build logs for security vulnerabilities using the ELK Stack.
   - Set up alerts for failed security tests or vulnerabilities discovered during dynamic security testing.

**Deliverables**:
- Jenkinsfile that integrates both SAST and DAST tools into the CI/CD pipeline.
- Ansible playbooks for secure configuration management using Ansible Vault.
- Monitoring and alert configurations to notify the team when vulnerabilities are detected.

---

**Solution**:

1. **Jenkinsfile**:
   - A Jenkins pipeline that integrates static and dynamic security testing (SonarQube, OWASP ZAP).
   - Notifications triggered when vulnerabilities are detected.

2. **Ansible Playbooks**:
   - Ansible playbooks using Vault to store sensitive information, such as credentials or API keys.

3. **ELK Monitoring**:
   - Configured Kibana dashboards and alerts that track build failures and security vulnerabilities.

---

### **Course Wrap-Up**:

By the end of this course, participants will have:
- Gained a deep understanding of **Agile, DevOps, and DevSecOps** principles.
- Mastered key tools like **Jenkins**, **Ansible**, **Puppet**, **Kubernetes**, **Terraform**, and the **ELK Stack**.
- Learned to automate key processes, including CI/CD pipelines, security testing, infrastructure provisioning, and continuous monitoring.
- Developed best practices for **secure development** and **infrastructure management** in real-world scenarios.

---

The final Q&A session will allow participants to ask any remaining questions, clarify concepts, or get feedback on their assignments. Participants should leave with confidence in applying these practices in their own DevOps environments.

