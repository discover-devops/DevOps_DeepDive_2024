### **Day 2: Development Environment & Version Control**

#### **Hour 3: Secure Version Control Practices**

---

#### **Concepts: Secure Code Storage, Branch Protection, Vulnerability Scanning**

In modern DevOps workflows, securing the version control process is critical to ensure that sensitive code and infrastructure configurations are not vulnerable to attacks. This hour focuses on various techniques for securing your Git workflows, including:

1. **Secure Code Storage**:
   - Store code in **private repositories** to limit access only to authorized personnel.
   - Implement **role-based access control** to manage who can read, write, or modify the repository.
   
2. **Branch Protection**:
   - Use **branch protection rules** to prevent unauthorized changes to critical branches (e.g., master, main).
   - Enforce policies like **requiring pull requests** before merging changes to sensitive branches and **mandating code reviews**.

3. **Vulnerability Scanning**:
   - Automate vulnerability scanning tools (e.g., **Snyk**, **Dependabot**) to detect vulnerabilities in dependencies or open-source libraries.
   - Use static code analysis tools (e.g., **SonarQube**) to identify security vulnerabilities early in the development cycle.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case for Branch Protection**:
   - In a critical application that handles sensitive customer data, an organization may enforce strict branch protection rules on the production branch. This ensures that no code is deployed to production without going through proper testing, reviews, and approval.

2. **Real-World Use Case for Vulnerability Scanning**:
   - A company using open-source libraries in their microservices application can integrate a tool like **Snyk** into their CI pipeline to automatically scan for known vulnerabilities whenever new dependencies are added or updated. If vulnerabilities are detected, the team is alerted and can patch them before they affect the application.

---

#### **Hands-on Lab 1: Securing Git Repositories with Branch Protection Rules**

**Scenario**:  
Participants will implement branch protection rules in a GitHub repository to prevent direct commits to the main branch and enforce code reviews.

##### **Lab Setup**:

1. **Step 1: Create a New Repository**
   - Go to GitHub and create a new repository.
   - Name the repository **secure-repo** and initialize it with a README.

2. **Step 2: Enable Branch Protection Rules**
   - Navigate to the repository's **Settings** → **Branches**.
   - Under the "Branch protection rules," click **Add Rule**.
   - Set the rule to apply to the **main** branch.
   - Enable the following options:
     - **Require pull request reviews before merging**: Ensure code is reviewed before being merged.
     - **Require status checks to pass before merging**: Integrate with CI/CD to ensure tests pass before merging.
     - **Require signed commits**: Enforce signing of commits to verify the identity of the contributor.

3. **Step 3: Test the Branch Protection Rules**
   - Try committing directly to the **main** branch. GitHub will block this action.
   - Create a feature branch:
     ```bash
     git checkout -b feature-secure
     ```
   - Make a change to `README.md` and commit the change.
     ```bash
     echo "Implementing security practices" >> README.md
     git add README.md
     git commit -m "Update README with security message"
     ```
   - Push the feature branch to GitHub:
     ```bash
     git push origin feature-secure
     ```
   - Open a **pull request** on GitHub to merge **feature-secure** into **main**.
   - Request a review and attempt to merge the pull request without approvals (it should be blocked due to the branch protection rule).

---

#### **Hands-on Lab 2: Two-Factor Authentication (2FA) for GitHub**

**Scenario**:  
Participants will enable and configure **two-factor authentication (2FA)** to add an extra layer of security to their GitHub accounts.

##### **Lab Setup**:

1. **Step 1: Enable 2FA on GitHub**
   - In your GitHub account, go to **Settings** → **Security**.
   - Under **Two-factor authentication**, click **Enable**.
   - Choose your authentication method (either **Authenticator App** or **SMS**).
   - Follow the steps to set up 2FA and verify it is working by logging out and logging back in.

2. **Step 2: Testing Git Operations with 2FA**
   - Once 2FA is enabled, try pushing code to a GitHub repository using HTTPS.
   - You will now be required to use a **personal access token** instead of your GitHub password.
     - Go to **Settings** → **Developer Settings** → **Personal Access Tokens**.
     - Generate a new token with appropriate scopes (e.g., repo, write).
   - Replace your password with the token when authenticating Git operations:
     ```bash
     git push origin feature-branch
     ```

3. **Step 3: Using SSH for Secure Access**
   - Alternatively, participants can set up **SSH keys** for GitHub:
     - Generate an SSH key:
       ```bash
       ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
       ```
     - Add the SSH key to your GitHub account under **Settings** → **SSH and GPG keys**.
     - Test the connection:
       ```bash
       ssh -T git@github.com
       ```

---

#### **Hands-on Lab 3: Automating Vulnerability Scanning with Snyk**

**Scenario**:  
Participants will integrate **Snyk** into their Git project to automatically scan for vulnerabilities in their dependencies.

##### **Lab Setup**:

1. **Step 1: Set Up a Sample Project**
   - Create a new project folder:
     ```bash
     mkdir secure-app && cd secure-app
     ```
   - Initialize a Git repository:
     ```bash
     git init
     ```
   - Add a sample `package.json` file (if working with Node.js) or another dependency file like `requirements.txt` (for Python):
     ```json
     {
       "name": "secure-app",
       "version": "1.0.0",
       "dependencies": {
         "express": "^4.17.1"
       }
     }
     ```
   - Commit the changes:
     ```bash
     git add .
     git commit -m "Add initial project with dependencies"
     ```

2. **Step 2: Install and Configure Snyk**
   - Install the Snyk CLI:
     ```bash
     npm install -g snyk
     ```
   - Authenticate with your Snyk account:
     ```bash
     snyk auth
     ```
   - Run a vulnerability scan on your project:
     ```bash
     snyk test
     ```
   - The output will show any vulnerabilities detected in your dependencies, including severity levels and potential fixes.

3. **Step 3: Fix Vulnerabilities**
   - Snyk can automatically suggest fixes for vulnerabilities. Run the following command to fix vulnerabilities in your dependencies:
     ```bash
     snyk wizard
     ```
   - After resolving the issues, commit the updated dependencies:
     ```bash
     git add .
     git commit -m "Fix vulnerabilities in dependencies"
     ```

4. **Step 4: Automating Snyk in CI Pipeline**
   - If you're using a CI/CD pipeline (e.g., Jenkins, GitLab CI), add **Snyk** as part of the pipeline to automatically scan for vulnerabilities with every commit.
   - For Jenkins, add a build step to run Snyk:
     ```bash
     snyk test
     ```

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Set up **branch protection rules** to secure critical branches and enforce code review policies.
- Configured **two-factor authentication (2FA)** for GitHub, securing their account and repository access.
- Integrated **Snyk** into their projects to automatically scan for vulnerabilities in dependencies and fix them proactively.

---

These labs provide a comprehensive hands-on experience with securing version control practices, branch protection, and vulnerability scanning—key elements of DevOps security. 
