### **Day 1: Agile Software Engineering & DevOps Fundamentals**

#### **Hour 6: Q&A and Recap of Day 1**

---

**Recap**: During this hour, I will go over the key concepts we have covered in Day 1, which include:

1. **Agile Methodologies**: The principles and values of Agile, as well as Scrum and Kanban frameworks.
2. **User Stories & Estimation Techniques**: How to write user stories, manage backlogs, and use estimation techniques like story points and planning poker.
3. **DevOps Culture and Practices**: The principles of DevOps and how to break down silos through collaboration.
4. **DevOps Lifecycle**: Mapping each stage of the DevOps value stream, from planning to monitoring.
5. **DevSecOps**: Integrating security into every stage of the DevOps lifecycle and automating security practices.

---

### **Assignments**

#### **Assignment 1: Mapping a DevOps Lifecycle for a New Project**

**Task**:  
- You are tasked with implementing DevOps practices for a new project: an e-commerce platform where users can browse products, add them to the cart, and check out.
- Map the **DevOps lifecycle** for this project, detailing each stage from planning to monitoring.
- Include the following:
  - How the **planning stage** will be managed (user stories, backlog management).
  - How the **code** will be developed, committed, and managed (version control, branching strategies).
  - How you will **build** and package the application (automating the build process).
  - What types of **testing** you will implement, focusing on security testing.
  - How you will **deploy** the application to production and manage environments.
  - How you will set up **monitoring** and feedback loops to improve the system continuously.

**Goal**: This assignment will test your understanding of mapping the DevOps lifecycle and integrating security into each stage.

---

#### **Assignment 2: Writing User Stories and Estimating Effort**

**Task**:  
- For the same e-commerce platform, write **5 user stories** that define the functionality of the project.
  - Example: "As a user, I want to be able to search for products so that I can quickly find items I want to buy."
- Estimate the **effort** required for each user story using **story points** (use the Fibonacci sequence: 1, 2, 3, 5, 8, 13).
- Apply the **planning poker** technique: briefly describe how you would discuss and decide on the final estimation with your team.

**Goal**: This assignment will help you practice writing user stories and applying Agile estimation techniques.

---

### **Solutions**

#### **Solution for Assignment 1: Mapping a DevOps Lifecycle for a New Project**

1. **Planning**: 
   - Use **Jira** to create and manage user stories and backlog items for the e-commerce platform.
   - Prioritize the backlog based on business value and customer needs.
  
2. **Coding**: 
   - Use **Git** as the version control system. 
   - Employ a **feature branching** strategy to ensure smooth collaboration between developers. 
   - Each feature branch is merged into the main branch after code reviews.

3. **Building**:
   - Automate the build process using **Jenkins** or **GitLab CI** to compile the code and package it into a Docker container.
   - Docker images are built using **Dockerfiles** and stored in a **Docker registry** for deployment.

4. **Testing**:
   - Integrate automated testing into the build pipeline. 
   - Perform **unit tests**, **integration tests**, and **static code analysis** using tools like **SonarQube** and **Snyk** for security vulnerabilities in dependencies.
   - Use **Selenium** for end-to-end testing.

5. **Deploying**:
   - Use **Jenkins** to automate the deployment process, deploying the Docker container to a **Kubernetes** cluster.
   - Implement **blue-green deployments** to reduce downtime and enable seamless transitions between releases.

6. **Monitoring**:
   - Set up **Prometheus** and **ELK Stack** (Elasticsearch, Logstash, and Kibana) for real-time monitoring of logs and system performance.
   - Integrate **AWS CloudWatch** or **Datadog** for infrastructure monitoring.
   - Set up alerts for security incidents and performance issues.

---

#### **Solution for Assignment 2: Writing User Stories and Estimating Effort**

1. **User Stories**:
   - **Story 1**: As a user, I want to be able to search for products so that I can quickly find items I want to buy. (Effort: **5 points**)
   - **Story 2**: As a user, I want to add items to my shopping cart so that I can purchase them later. (Effort: **3 points**)
   - **Story 3**: As a user, I want to checkout using various payment methods so that I can complete my purchase. (Effort: **8 points**)
   - **Story 4**: As a user, I want to view my order history so that I can track previous purchases. (Effort: **3 points**)
   - **Story 5**: As an admin, I want to manage the inventory of products so that I can ensure products are in stock. (Effort: **8 points**)

2. **Planning Poker**:
   - Each team member assigns story points based on their understanding of the effort required.
   - After the first round of estimates, any significant differences are discussed. For example, if one member assigns 8 points while others assign 3, they will explain their reasoning (perhaps the complexity of the checkout system was underestimated).
   - After the discussion, the team comes to a consensus on the final story point estimate, ensuring that everyone has considered the full scope of the work.

---

