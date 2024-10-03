### **Day 4: Continuous Integration, Testing & Orchestration**

#### **Hour 1: Introduction to Continuous Integration (CI)**

---

#### **Concepts: Continuous Integration Principles and Jenkins Setup**

**Continuous Integration (CI)** is a development practice where developers frequently merge code into a shared repository, multiple times a day. Each check-in is automatically tested, ensuring that bugs are caught early and code remains in a deployable state.

**CI Principles**:
- **Frequent commits**: Developers commit their changes frequently, ideally multiple times per day, which reduces integration problems.
- **Automated builds**: Every code commit triggers an automated build, ensuring that the code compiles and passes tests.
- **Automated testing**: Running automated tests to verify that the new changes don’t break existing functionality.

**Key Benefits of CI**:
- **Faster feedback**: CI provides quick feedback on the impact of changes.
- **Reduced integration problems**: By integrating code continuously, developers avoid large-scale merge conflicts or bugs.
- **Higher code quality**: Automated testing ensures that bugs are caught early, leading to more stable code.

---

**Jenkins Overview and Setup**:

**Jenkins** is a popular open-source automation server that supports building, deploying, and automating projects. It is widely used for **CI/CD** pipelines.

- **Jenkins Master-Slave Architecture**: The Jenkins master is responsible for managing jobs, while the slave nodes (agents) execute the jobs.
- **Plugins**: Jenkins has a rich set of plugins for integrating with version control systems (Git), build tools (Maven, Gradle), containerization tools (Docker), and cloud platforms (AWS, Azure).

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Automated Builds for a Microservices Application**:
   - A company with a microservices architecture uses **Jenkins** to automate the build and testing process for each microservice. Every time a developer commits changes to the code repository, Jenkins triggers a build, runs unit tests, and notifies the developer if there are any issues, ensuring quick feedback and stable releases.

2. **Real-World Use Case: Continuous Integration for Mobile Apps**:
   - A mobile app development team uses **Jenkins** to automate their builds. Every time code is pushed to the repository, Jenkins builds the iOS and Android versions of the app, runs tests, and produces signed binaries for distribution.

---

#### **Hands-on Lab: Configure a Jenkins Server and Create a Simple CI Pipeline**

In this lab, participants will install and configure Jenkins, create a simple **CI pipeline**, and trigger builds automatically on code changes.

---

### **Lab 1: Installing Jenkins and Setting Up a CI Pipeline**

**Prerequisites**:
- A Linux-based server (e.g., Ubuntu 20.04) for Jenkins installation.
- Java installed on the server (Jenkins requires Java).
- Basic knowledge of Git.

---

**Step 1: Install Jenkins on an Ubuntu Server**

1. **Install Java**:
   Jenkins requires **Java** to run. Install it by running:
   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk -y
   ```

2. **Add the Jenkins Repository**:
   Add the Jenkins package repository to your system:
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

3. **Install Jenkins**:
   After adding the repository, install Jenkins:
   ```bash
   sudo apt update
   sudo apt install jenkins -y
   ```

4. **Start Jenkins**:
   Start Jenkins and enable it to start at boot:
   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```

5. **Access Jenkins**:
   - Open a web browser and go to `http://your_server_ip:8080`.
   - To unlock Jenkins, use the initial admin password stored in the file:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

6. **Install Suggested Plugins**:
   After unlocking Jenkins, it will ask you to install plugins. Choose "Install suggested plugins" to get the commonly used plugins.

---

**Step 2: Configure Jenkins and Create Your First Job**

1. **Configure Jenkins**:
   - Create your first admin user by following the setup wizard.
   - Configure the basic settings (e.g., Jenkins URL, email notifications).

2. **Install Git Plugin**:
   - Go to **Manage Jenkins** → **Manage Plugins** and install the **Git plugin** to allow Jenkins to integrate with Git repositories.

---

**Step 3: Create a Simple CI Pipeline Job**

1. **Create a New Jenkins Job**:
   - In Jenkins, click on **New Item** and choose **Freestyle Project**.
   - Name your project **Simple-CI-Pipeline**.

2. **Configure the Job**:
   - Under the **Source Code Management** section, select **Git** and enter the URL of your GitHub repository:
     ```bash
     https://github.com/yourusername/yourproject.git
     ```
   - If your repository is private, provide the necessary credentials.

3. **Add Build Steps**:
   - Under **Build**, click **Add build step** and select **Execute Shell**.
   - Add a simple build command. For example, if your project is in **Python**, you can add:
     ```bash
     echo "Running CI Build"
     python3 -m unittest discover
     ```

---

**Step 4: Trigger Builds Automatically on Code Changes**

1. **Configure Polling**:
   - Under the **Build Triggers** section, select **Poll SCM** and set the schedule to check for changes (e.g., every 5 minutes):
     ```bash
     H/5 * * * *
     ```

2. **Save and Build**:
   - Save the job and click **Build Now** to manually trigger a build.
   - Jenkins will fetch the latest code, execute the build steps, and display the results in the **Console Output**.

---

### **Lab 2: Creating a Jenkins Pipeline Using a Jenkinsfile**

In this lab, participants will use a **Jenkinsfile** to define the CI pipeline in a declarative format.

---

**Step 1: Create a `Jenkinsfile` in Your Repository**

1. In your project repository, create a file named `Jenkinsfile` with the following content:
   ```groovy
   pipeline {
       agent any

       stages {
           stage('Checkout') {
               steps {
                   git 'https://github.com/yourusername/yourproject.git'
               }
           }

           stage('Build') {
               steps {
                   echo 'Building the application...'
                   sh 'echo Running Build'
               }
           }

           stage('Test') {
               steps {
                   echo 'Running tests...'
                   sh 'python3 -m unittest discover'
               }
           }
       }

       post {
           success {
               echo 'Build and Test completed successfully!'
           }
           failure {
               echo 'Build or Test failed.'
           }
       }
   }
   ```

---

**Step 2: Configure the Jenkins Job for Pipeline**

1. In Jenkins, create a **new pipeline job**.
2. Under the **Pipeline** section, choose **Pipeline script from SCM**.
3. Choose **Git** as the SCM and provide the repository URL.
4. Jenkins will look for the **Jenkinsfile** and execute the pipeline stages defined in the file.

---

**Step 3: Run the Jenkins Pipeline**

1. Trigger the pipeline by clicking **Build Now**. Jenkins will:
   - Clone the repository.
   - Execute the build and test stages defined in the Jenkinsfile.
   - Provide feedback on success or failure.

---

### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Installed and configured Jenkins on a Linux server.
- Created a simple Jenkins job that automatically builds code from a Git repository.
- Used a **Jenkinsfile** to define a more advanced pipeline for CI.

---

These labs provide hands-on experience with Jenkins, helping participants understand how to set up CI pipelines, automate builds, and trigger builds based on code changes. 
