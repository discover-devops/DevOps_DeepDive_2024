### **Day 2: Development Environment & Version Control**

#### **Hour 6: Q&A and Recap of Day 2**

---

**Recap**: During this hour, I will review the key topics covered in Day 2, which include:

1. **Setting Up Development Environments**: Installing Git, setting up a local development environment with VS Code, and configuring version control.
2. **Git Branching and Merging Strategies**: How to create branches, merge changes, and resolve conflicts using Git.
3. **Secure Version Control Practices**: Applying secure practices in version control, including branch protection, 2FA, and vulnerability scanning.
4. **Introduction to Docker**: Benefits of containerization, Docker architecture, and Dockerfile basics.
5. **Running a Containerized Application**: Docker commands, managing containers, and port mapping to run applications in Docker.

---

### **Assignments**

#### **Assignment 1: Implement Secure Git Practices in a Real-World Project**

**Scenario**:  
You are part of a DevOps team responsible for developing and maintaining a web application that handles sensitive customer data. As part of your responsibility, you need to implement secure version control practices to ensure that only reviewed and secure code is deployed to production.

**Tasks**:
1. **Create a New Repository**: Set up a private GitHub repository for the project.
2. **Enable Branch Protection**:
   - Protect the **main** branch to ensure that no code is committed directly without a pull request.
   - Enforce rules like requiring code reviews and successful status checks before merging.
3. **Enable Two-Factor Authentication (2FA)**: Ensure that every team member has enabled 2FA on their GitHub accounts.
4. **Run Vulnerability Scans**: Integrate a vulnerability scanning tool (e.g., Snyk, Dependabot) to automatically scan your project for security vulnerabilities. Fix any issues that arise and document your findings.
5. **Create Branches and Submit Pull Requests**:
   - Create a **feature branch** for a new functionality (e.g., adding a login feature).
   - After completing the feature, submit a pull request to merge the changes into the **main** branch.
   - Ensure the pull request is reviewed by a teammate and passes all checks before merging.

**Goal**: This assignment will simulate real-world Git usage in a production environment, reinforcing secure practices while managing code changes effectively.

---

#### **Assignment 2: Containerizing and Deploying a Web Application**

**Scenario**:  
You have been assigned to containerize a simple web application and deploy it on your organization's development server. You need to use Docker to ensure the application runs consistently across all environments and make it accessible to external users via port mapping.

**Tasks**:
1. **Create a Simple Web Application**:
   - Write a basic **Node.js** or **Python Flask** web application that returns a "Hello World" message.
2. **Write a Dockerfile**:
   - Create a Dockerfile to containerize the application.
   - Ensure the Dockerfile installs all necessary dependencies and exposes the required port (e.g., 80 or 8080).
3. **Build and Run the Docker Container**:
   - Build a Docker image from the Dockerfile.
   - Run the container, mapping the internal application port to an external port (e.g., 4000:80).
   - Access the application in a web browser to ensure it works.
4. **Create a Docker Compose Setup**:
   - Extend the project to include a second container (e.g., an NGINX reverse proxy or a database like MySQL).
   - Write a **docker-compose.yml** file to manage both services.
5. **Run and Test**:
   - Use Docker Compose to build and run both containers.
   - Verify that the web application and the reverse proxy (or database) are running correctly and are accessible.

**Goal**: This assignment will provide hands-on experience with containerizing applications using Docker and managing multi-container setups using Docker Compose, simulating real-world deployment tasks.

---

These assignments reflect real-world scenarios that DevOps teams often encounter, ensuring participants gain practical experience with secure version control and containerized deployments. 
