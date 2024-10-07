### **Day 4: Continuous Integration, Testing & Orchestration**

#### **Hour 6: Q&A and Recap of Day 4**

---

**Recap**: During this hour, I will summarize the key topics covered throughout **Day 4**, which include:

1. **Introduction to Continuous Integration (CI)**: Understanding CI principles, setting up Jenkins, and creating simple CI pipelines.
2. **Automating Testing in Jenkins**: How to automate builds and unit tests, and integrate security tests into Jenkins.
3. **Selenium for Secure Testing**: Writing secure Selenium tests to identify vulnerabilities in web applications, such as form validation and XSS.
4. **Introduction to Kubernetes**: Kubernetes basics, including Pods, Deployments, and Services, and deploying a containerized app on a local Kubernetes cluster.
5. **Container Orchestration and Security in Kubernetes**: Exploring Kubernetes security features, such as RBAC and Network Policies, to secure a Kubernetes environment.

---

### **Assignments**

#### **Assignment 1: Automate a Jenkins Pipeline with Unit Testing and Notifications**

**Scenario**:  
Your organization requires you to automate the testing of a simple Python application in Jenkins. Additionally, you need to set up notifications to inform the team about the build status. You will automate a **CI pipeline** in Jenkins that runs **unit tests** on every commit and notifies the team via email if the build fails.

---

**Tasks**:

1. **Set up a Jenkins Job**:
   - Create a **Pipeline Job** in Jenkins that pulls code from your GitHub repository.
   - Add a **build step** that installs the necessary dependencies and runs unit tests for your Python application.

2. **Configure Jenkins for Email Notifications**:
   - Configure **Jenkins Email Notification** using an SMTP server to send emails when a build fails.

3. **Define the Jenkins Pipeline**:
   - Write a **Jenkinsfile** that:
     - Pulls the code from GitHub.
     - Installs the Python dependencies.
     - Runs the unit tests.
     - Sends an email notification to the team if the build fails.

**Deliverables**:
- The **Jenkinsfile** defining the pipeline.
- A screenshot of the Jenkins job output showing successful or failed test execution.
- Email configuration and proof of email notification (optional).

---

**Solution**:

1. **Jenkinsfile**:
   ```groovy
   pipeline {
       agent any

       stages {
           stage('Checkout') {
               steps {
                   git 'https://github.com/yourusername/sample-python-app.git'
               }
           }

           stage('Install Dependencies') {
               steps {
                   sh 'pip install -r requirements.txt'
               }
           }

           stage('Run Unit Tests') {
               steps {
                   sh 'python3 -m unittest discover'
               }
           }
       }

       post {
           failure {
               mail to: 'team@example.com',
                    subject: "Build Failed: ${currentBuild.fullDisplayName}",
                    body: "Something is wrong with the build ${env.BUILD_URL}"
           }
           success {
               echo 'Build Passed!'
           }
       }
   }
   ```

2. **Email Notification Setup**:
   - In Jenkins, go to **Manage Jenkins** → **Configure System** → **E-mail Notification**.
   - Enter your SMTP server details and configure email notifications for build failures.

---

#### **Assignment 2: Implement Kubernetes Security with RBAC and Network Policies**

**Scenario**:  
Your organization has a Kubernetes cluster running multiple microservices. You need to implement security measures to restrict access to Kubernetes resources and control network communication between the microservices. You will configure **RBAC** to limit access to Pods and apply **Network Policies** to secure network traffic.

---

**Tasks**:

1. **Implement Role-Based Access Control (RBAC)**:
   - Create a **Service Account** for a user with limited access to Pods.
   - Create a **Role** that allows read-only access to Pods in the `default` namespace.
   - Create a **RoleBinding** to bind the role to the service account.

2. **Apply Network Policies**:
   - Define a **Network Policy** to allow traffic only between specific microservices (e.g., frontend and backend Pods).
   - Ensure that other Pods cannot communicate with the backend Pod.

3. **Test the Security Configuration**:
   - Verify that the service account has read-only access to Pods.
   - Test the network policy by deploying test Pods and ensuring only authorized communication occurs.

**Deliverables**:
- The YAML files for the **Role**, **RoleBinding**, and **Network Policy**.
- A brief explanation of the security configuration.
- A screenshot showing the service account's restricted access and network policy enforcement.

---

**Solution**:

1. **Role YAML** (`role.yml`):
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     namespace: default
     name: pod-reader
   rules:
   - apiGroups: [""]
     resources: ["pods"]
     verbs: ["get", "list"]
   ```

2. **RoleBinding YAML** (`rolebinding.yml`):
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: RoleBinding
   metadata:
     name: pod-read-access
     namespace: default
   subjects:
   - kind: ServiceAccount
     name: devops-sa
     namespace: default
   roleRef:
     kind: Role
     name: pod-reader
     apiGroup: rbac.authorization.k8s.io
   ```

3. **Network Policy YAML** (`network-policy.yml`):
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: backend-policy
     namespace: default
   spec:
     podSelector:
       matchLabels:
         app: backend
     policyTypes:
     - Ingress
     ingress:
     - from:
       - podSelector:
           matchLabels:
             app: frontend
     ports:
     - protocol: TCP
       port: 5000
   ```

4. **Testing**:
   - Verify that the service account can list Pods but cannot delete them.
   - Deploy frontend and backend Pods and confirm that the network policy only allows communication between the frontend and backend.

---

### **Lab Wrap-Up**

By the end of these assignments, participants will have:
- Automated a Jenkins CI pipeline with unit testing and email notifications.
- Implemented Kubernetes security features using RBAC and network policies to control access and communication.

These assignments reinforce the practical skills learned during **Day 4** and help participants apply security best practices in real-world DevOps environments. 
