### **Day 4: Continuous Integration, Testing & Orchestration**

#### **Hour 2: Automating Testing in Jenkins**

---

#### **Concepts: Automating Builds and Tests in Jenkins**

Automating testing in a **CI/CD pipeline** is critical to ensuring that code is continuously verified before it is merged into the main branch or deployed into production. Jenkins provides a robust platform for automating both **builds** and **tests** across multiple stages of a pipeline.

Key components of automated testing in Jenkins:
- **Unit Tests**: Tests for individual components or functions of your application to ensure they work as expected.
- **Integration Tests**: Tests that check how different components work together.
- **Security Tests**: Ensure that the code is free of vulnerabilities, commonly done with static analysis tools or security scanners.
- **Test Reports**: Automatically generating reports that summarize the results of the tests and notifying developers if any issues are detected.

---

**Real-World Benefits**:
- **Consistency**: By automating the testing process, you ensure that all code changes are tested in a consistent manner.
- **Faster Feedback**: Developers get quick feedback about the impact of their changes, reducing the time it takes to fix bugs.
- **Increased Code Quality**: Continuous testing improves code quality by catching issues early in the development lifecycle.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Automating Unit Tests for a Web Application**:
   - A company developing a web application automates its unit tests using Jenkins. Every time a developer pushes changes to the repository, Jenkins runs the tests, checking for any issues in the business logic. This ensures that new features don’t break existing functionality.

2. **Automating Security Tests for a DevOps Pipeline**:
   - A team uses Jenkins to run security tests on their codebase. Tools like **SonarQube** or **OWASP ZAP** are integrated into the pipeline to detect common vulnerabilities, such as SQL injection or cross-site scripting (XSS). The pipeline halts if security flaws are detected, ensuring only secure code moves to production.

---

#### **Hands-on Lab: Automate a Sample Build with Unit Tests**

In this lab, participants will automate the testing process for a simple Python application by configuring Jenkins to execute unit tests and report on test results.

---

### **Lab 1: Automating Unit Tests in Jenkins**

**Prerequisites**:
- Jenkins is installed and configured (from Hour 1 lab).
- A sample Python project with unit tests.
- Basic knowledge of Python and Git.

---

**Step 1: Create a Simple Python Application with Unit Tests**

1. **Create a Python project**:
   - On your local machine, create a directory for your Python project:
     ```bash
     mkdir sample-python-app && cd sample-python-app
     ```

2. **Create a simple Python script** (`app.py`):
   ```python
   def add(x, y):
       return x + y

   def subtract(x, y):
       return x - y
   ```

3. **Create a unit test file** (`test_app.py`):
   ```python
   import unittest
   from app import add, subtract

   class TestApp(unittest.TestCase):
       def test_add(self):
           self.assertEqual(add(2, 3), 5)

       def test_subtract(self):
           self.assertEqual(subtract(5, 3), 2)

   if __name__ == '__main__':
       unittest.main()
   ```

4. **Add a requirements file** (`requirements.txt`):
   ```text
   unittest
   ```

5. **Initialize a Git repository**:
   - Initialize a Git repository and push your code to GitHub (or any Git service):
     ```bash
     git init
     git add .
     git commit -m "Initial commit with unit tests"
     git remote add origin https://github.com/yourusername/sample-python-app.git
     git push -u origin master
     ```

---

**Step 2: Configure Jenkins to Automate Unit Tests**

1. **Create a Jenkins Job**:
   - In Jenkins, create a **Freestyle Project** and name it **Sample-Python-Test**.
   - Under **Source Code Management**, choose **Git** and provide the repository URL:
     ```bash
     https://github.com/yourusername/sample-python-app.git
     ```
   - Add any necessary credentials if the repository is private.

2. **Add Build Steps**:
   - Under the **Build** section, add the following steps:
     - **Execute Shell**:
       ```bash
       # Set up Python environment
       virtualenv venv
       source venv/bin/activate

       # Install dependencies
       pip install -r requirements.txt

       # Run the unit tests
       python3 -m unittest discover
       ```

---

**Step 3: Generate and Display Test Results in Jenkins**

1. **Publish JUnit Test Results**:
   - Jenkins can automatically parse the output of the **unittest** framework and generate a report.
   - Install the **JUnit plugin** if it is not already installed.
   - Go to the job configuration page, scroll down to the **Post-build Actions** section, and choose **Publish JUnit test result report**.
   - In the **Test Report XMLs** field, enter:
     ```bash
     **/test-reports/*.xml
     ```

2. **Generate JUnit Test Report in Python**:
   - Modify the `unittest` command in the **build step** to generate a **JUnit XML** report:
     ```bash
     python3 -m unittest discover -v | tee result.log
     python3 -m junitparser --output test-reports/test-results.xml result.log
     ```

   This step assumes that you are using a plugin like **junitparser** to convert test logs to JUnit-compatible XML format.

3. **Run the Job**:
   - Click **Build Now** to trigger the job.
   - Jenkins will pull the code, run the tests, and publish the test results in the Jenkins interface.

---

### **Lab 2: Automating Security Tests with Jenkins and OWASP ZAP**

In this lab, participants will integrate **OWASP ZAP** (a security scanner) into Jenkins to automate basic security testing for their web application.

---

**Step 1: Install OWASP ZAP**

1. On your local machine or Jenkins server, install **OWASP ZAP**:
   ```bash
   sudo apt install zaproxy
   ```

2. Start OWASP ZAP with the following command:
   ```bash
   zaproxy &
   ```

---

**Step 2: Configure Jenkins to Run OWASP ZAP Tests**

1. **Install the ZAP Plugin**:
   - Go to **Manage Jenkins** → **Manage Plugins**, search for the **OWASP ZAP Plugin**, and install it.

2. **Create a Jenkins Job for Security Testing**:
   - In Jenkins, create a **Freestyle Project** and name it **Security-Test**.
   - Under **Build Triggers**, choose **Build after other projects are built**, and specify the CI job created earlier (from Hour 1).

3. **Add OWASP ZAP as a Build Step**:
   - In the job configuration page, under **Build**, add a new step for **OWASP ZAP** and configure it to scan your application:
     - Set the **ZAP Target URL** to the URL of your application (if it’s a web app running locally or on a server).
     - Configure other options, such as **passive scan**, **active scan**, or **spider scan**.

4. **Add a Post-Build Step**:
   - Add a **Publish OWASP ZAP Reports** action to publish the results of the security scan.

5. **Run the Job**:
   - Trigger the security test job after the build completes. Jenkins will execute the OWASP ZAP scan and generate a report of any security vulnerabilities detected.

---

### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Configured Jenkins to automate the testing of a Python project with **unit tests**.
- Integrated **JUnit test results** into Jenkins, generating automated test reports.
- Set up Jenkins to run **OWASP ZAP** for basic security testing and published the results.

---

These labs provide participants with practical experience in automating testing workflows, both for unit tests and security tests, within Jenkins. 
