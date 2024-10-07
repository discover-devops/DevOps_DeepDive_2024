### **Day 4: Continuous Integration, Testing & Orchestration**

#### **Hour 3: Selenium for Secure Testing**

---

#### **Concepts: Writing Secure Selenium Tests for Web Applications**

**Selenium** is a popular open-source tool for automating web browsers. It is commonly used for automated testing of web applications, including functional, integration, and security tests. While Selenium is primarily used for functional tests, it can also be used to detect certain types of vulnerabilities, such as cross-site scripting (XSS) or input validation issues.

---

**Secure Testing with Selenium**:
- **Functional Security Testing**: Ensures that security mechanisms like login forms, password fields, and input validation work as expected.
- **Cross-Site Scripting (XSS) Detection**: Testing if web applications are vulnerable to XSS by injecting scripts into input fields and verifying that they are properly sanitized.
- **Form and Field Validation**: Ensure that forms and input fields are properly validated to prevent malicious data from being submitted.
- **SSL/TLS Certificate Validation**: Verify that the web application is served over HTTPS and uses valid certificates.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Validating User Authentication and Session Management**:
   - A company uses **Selenium** to test the login functionality of its web application. The test script ensures that unauthorized users are prevented from accessing secure pages and that sessions are managed securely (e.g., logout invalidates the session).
   
2. **Real-World Use Case: Detecting Cross-Site Scripting (XSS) Vulnerabilities**:
   - A security team uses Selenium to simulate malicious input, attempting to inject JavaScript into forms on a web application. They verify that the application properly sanitizes the input to protect against XSS attacks.

---

#### **Hands-on Lab: Write Selenium Scripts to Test an Application for Vulnerabilities**

In this lab, participants will use **Selenium WebDriver** to write automated scripts that test a sample web application for security vulnerabilities, focusing on form validation, XSS prevention, and secure login testing.

---

### **Lab 1: Setting Up Selenium for Automated Secure Testing**

**Prerequisites**:
- **Selenium WebDriver** installed.
- A web application to test (can be a locally hosted or publicly available app).
- **Python** and **pip** installed for writing the Selenium scripts.

---

**Step 1: Install Selenium and WebDriver**

1. **Install Selenium**:
   - On your local machine, install Selenium using `pip`:
     ```bash
     pip install selenium
     ```

2. **Download WebDriver**:
   - Depending on the browser you wish to automate (Chrome, Firefox, etc.), download the corresponding WebDriver:
     - **Chrome WebDriver**: [Download from here](https://sites.google.com/a/chromium.org/chromedriver/downloads)
     - **GeckoDriver** for Firefox: [Download from here](https://github.com/mozilla/geckodriver/releases)
   - After downloading the WebDriver, place it in a directory and add it to your system's PATH.

---

**Step 2: Write a Selenium Script to Test Form Validation and XSS**

In this step, you will write a Selenium script that interacts with a sample login form, tests form validation, and attempts to inject malicious scripts to check for XSS vulnerabilities.

1. **Create a Python file** (`test_form_security.py`) and add the following code:

   ```python
   from selenium import webdriver
   from selenium.webdriver.common.keys import Keys
   from selenium.webdriver.common.by import By
   import time

   # Start the WebDriver (adjust path to your ChromeDriver or GeckoDriver)
   driver = webdriver.Chrome(executable_path='/path/to/chromedriver')

   # Open the web application
   driver.get("http://example.com/login")

   # Ensure the page title is correct
   assert "Login" in driver.title

   # Test case: Valid login
   username_field = driver.find_element(By.NAME, "username")
   password_field = driver.find_element(By.NAME, "password")
   submit_button = driver.find_element(By.NAME, "login")

   # Input valid credentials
   username_field.send_keys("validUser")
   password_field.send_keys("validPassword")
   submit_button.click()

   # Verify successful login by checking for a specific element on the homepage
   try:
       welcome_message = driver.find_element(By.ID, "welcome")
       print("Login Test Passed: Welcome message displayed")
   except:
       print("Login Test Failed")

   # Test case: XSS attack simulation
   driver.get("http://example.com/comment")

   # Input malicious script in the comment field
   comment_field = driver.find_element(By.NAME, "comment")
   comment_field.send_keys("<script>alert('XSS')</script>")
   submit_button = driver.find_element(By.NAME, "submit")
   submit_button.click()

   # Check for alert (which would indicate XSS vulnerability)
   try:
       alert = driver.switch_to.alert
       print("XSS Vulnerability Detected")
       alert.accept()
   except:
       print("XSS Test Passed: No vulnerability detected")

   # Close the browser
   driver.quit()
   ```

---

**Step 3: Run the Selenium Script**

1. In your terminal or command prompt, run the script:
   ```bash
   python3 test_form_security.py
   ```

2. The script will:
   - Open the web application.
   - Test the login functionality by entering valid credentials.
   - Attempt to inject a JavaScript script into a form and check for XSS vulnerabilities.
   - Output the results to the console, indicating whether the form was vulnerable to XSS or if the login succeeded.

---

### **Lab 2: Selenium Testing for Secure Login and SSL/TLS Validation**

This lab focuses on testing the secure login mechanism of a web application, ensuring that it follows best practices for user authentication and that the application uses valid SSL/TLS certificates.

---

**Step 1: Write a Selenium Script for Secure Login Testing**

1. **Create a Python file** (`test_secure_login.py`) and add the following code:

   ```python
   from selenium import webdriver
   from selenium.webdriver.common.by import By
   import time

   # Start the WebDriver (adjust path to your ChromeDriver or GeckoDriver)
   driver = webdriver.Chrome(executable_path='/path/to/chromedriver')

   # Open the login page
   driver.get("https://secure.example.com/login")

   # Ensure the page is served over HTTPS
   if "https" in driver.current_url:
       print("SSL Test Passed: Site is using HTTPS")
   else:
       print("SSL Test Failed: Site is not using HTTPS")

   # Test case: Invalid login
   username_field = driver.find_element(By.NAME, "username")
   password_field = driver.find_element(By.NAME, "password")
   submit_button = driver.find_element(By.NAME, "login")

   # Input invalid credentials
   username_field.send_keys("invalidUser")
   password_field.send_keys("invalidPassword")
   submit_button.click()

   # Check for login failure message
   try:
       error_message = driver.find_element(By.ID, "error-message")
       print("Invalid Login Test Passed: Error message displayed")
   except:
       print("Invalid Login Test Failed: Error message not found")

   # Close the browser
   driver.quit()
   ```

---

**Step 2: Run the Secure Login Test**

1. In your terminal, run the secure login test script:
   ```bash
   python3 test_secure_login.py
   ```

2. The script will:
   - Open the secure login page.
   - Check whether the page is served over HTTPS (indicating SSL/TLS protection).
   - Test login with invalid credentials and check for appropriate error messages.

---

### **Lab 3: Integrating Selenium Tests into Jenkins for Continuous Security Testing**

Now that we have written our **Selenium tests**, let’s automate the execution of these tests in a Jenkins pipeline to continuously test our web applications for security vulnerabilities.

---

**Step 1: Create a Jenkins Pipeline with Selenium Tests**

1. **Jenkinsfile**:
   - Create a `Jenkinsfile` in your repository that defines the pipeline stages for running the Selenium tests:
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
                     sh '''
                     pip install selenium
                     '''
                 }
             }

             stage('Run Selenium Tests') {
                 steps {
                     sh '''
                     python3 test_form_security.py
                     python3 test_secure_login.py
                     '''
                 }
             }
         }

         post {
             always {
                 echo 'Tests Completed'
             }
         }
     }
     ```

2. **Configure Jenkins Job**:
   - Create a new **Pipeline Job** in Jenkins and configure it to pull from your repository.
   - Jenkins will execute the pipeline stages, running the Selenium scripts as part of the build process.

3. **Run the Pipeline**:
   - Trigger the Jenkins pipeline, and it will run the Selenium tests and output the results in the Jenkins console.

---

### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Written **Selenium scripts** to test for form validation, secure login mechanisms, and XSS vulnerabilities.
- Integrated these tests into a **Jenkins pipeline** for continuous security testing.
- Learned how to automate the detection of security vulnerabilities using Selenium.

---

These labs provide hands-on experience in writing secure tests with Selenium and automating them in Jenkins to ensure that web applications are tested continuously for vulnerabilities. 
