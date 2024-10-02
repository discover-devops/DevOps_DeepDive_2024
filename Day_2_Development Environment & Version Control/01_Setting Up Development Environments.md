### **Day 2: Development Environment & Version Control**

#### **Hour 1: Setting Up Development Environments**

---

#### **Concepts: IDE Configuration & Version Control Basics**

In this session, we will focus on setting up a development environment, configuring an Integrated Development Environment (IDE), and understanding version control systems like **Git**. The objective is to ensure that participants are comfortable with their tools and understand how version control fits into the DevOps workflow.

---

#### **IDE Configuration (VS Code)**

**Visual Studio Code (VS Code)** is one of the most popular IDEs used in DevOps and software development. It is lightweight, fast, and highly customizable through plugins. Here's a brief setup guide:

1. **Install VS Code**: Participants should download and install VS Code from the official [VS Code website](https://code.visualstudio.com/).

2. **Customize the Environment**: Once installed, encourage participants to install the following useful extensions for a smoother DevOps workflow:
   - **GitLens**: To enhance Git integration and provide detailed Git insights.
   - **Docker**: For working with Docker containers and images.
   - **Python** (or relevant programming language extensions): Useful for developers writing Python scripts, or similar extensions for other languages (Node.js, Java, etc.).
   - **Live Share**: For real-time collaboration on code with other team members.

3. **Customizing Workspace Settings**: Instruct participants to customize their settings (font size, color theme, shortcuts) to match their preferences, enhancing their productivity.

---

#### **Version Control Basics (Git)**

**Git** is a distributed version control system that allows teams to track code changes, collaborate on code, and manage versions of a project. It is a fundamental tool for DevOps workflows.

- **Key Concepts**:
  - **Repository (Repo)**: A project folder where all files and their history are stored.
  - **Commit**: A snapshot of changes in the project.
  - **Branch**: An independent line of development. Teams often use branches for different features.
  - **Merge**: Combining changes from different branches into one.
  - **Pull/Push**: Syncing your local code with a remote repository (like GitHub).

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case for Git**:
   - Imagine a DevOps team working on a microservices architecture, where each developer is responsible for a specific microservice. Using Git, each developer can work on their microservice independently by creating a separate branch. Once their changes are tested, they merge their code back into the main branch, enabling smooth and isolated development.

2. **Real-World Use Case for IDEs**:
   - In a cloud-native environment, developers often integrate directly with cloud providers like AWS or Azure via their IDE. VS Code offers plugins that allow developers to connect to AWS Lambda functions, Kubernetes clusters, or Azure web apps, making it easier to interact with cloud infrastructure during development.

---

#### **Hands-on Lab: Install Git and Set Up a Local Development Environment**

##### **Lab Setup**:

**Prerequisites**:
- Participants should have access to their laptops/desktops (Linux, macOS, or Windows).

---

**Step 1: Install Git**

1. **Windows**:
   - Download Git for Windows from [Git for Windows](https://git-scm.com/).
   - Install using the default settings.

2. **macOS**:
   - Open the terminal and run the command:
     ```bash
     brew install git
     ```

3. **Linux**:
   - Use your package manager:
     ```bash
     sudo apt install git   # For Ubuntu/Debian
     sudo yum install git   # For CentOS/RHEL
     ```

**Verification**:
- After installation, verify Git by typing the following command in the terminal:
  ```bash
  git --version
  ```
  This should return the installed version of Git.

---

**Step 2: Set Up Git Configuration**

Once Git is installed, participants need to configure Git with their personal information. This is essential for tracking commits.

1. Configure your name:
   ```bash
   git config --global user.name "Your Name"
   ```

2. Configure your email address:
   ```bash
   git config --global user.email "your.email@example.com"
   ```

3. Confirm the settings:
   ```bash
   git config --list
   ```

---

**Step 3: Install Visual Studio Code**

1. Download and install VS Code from the [official website](https://code.visualstudio.com/).
2. Launch VS Code and install the necessary extensions:
   - **GitLens**: Provides powerful Git features integrated into VS Code.
   - **Docker** (if applicable).
   - **Language extensions** (Python, Node.js, etc.) depending on your development stack.

---

**Step 4: Initialize a Git Repository in VS Code**

1. Open VS Code and create a new folder for your project.
2. In the VS Code terminal, navigate to the folder you just created and initialize it as a Git repository:
   ```bash
   git init
   ```
3. Create a sample file, for example, `README.md`:
   ```bash
   echo "# My DevOps Project" > README.md
   ```
4. Add the file to the Git staging area:
   ```bash
   git add README.md
   ```
5. Commit the file:
   ```bash
   git commit -m "Initial commit with README"
   ```
6. Connect to a remote GitHub repository (if applicable):
   - Create a new repository on [GitHub](https://github.com/).
   - Add the remote link:
     ```bash
     git remote add origin https://github.com/yourusername/yourrepository.git
     ```
   - Push your commit to GitHub:
     ```bash
     git push -u origin master
     ```

---

**Step 5: Basic Git Commands Practice**

Participants should practice basic Git commands using their initialized Git repository:

1. Make changes to the `README.md` file.
2. Add the changes:
   ```bash
   git add README.md
   ```
3. Commit the changes:
   ```bash
   git commit -m "Updated README with new content"
   ```
4. View the commit history:
   ```bash
   git log
   ```
5. Create a new branch:
   ```bash
   git checkout -b feature-branch
   ```
6. Merge the new branch into the main branch:
   ```bash
   git checkout master
   git merge feature-branch
   ```

---

#### **Lab Wrap-Up**

By the end of this lab, participants will have:
- Installed and configured Git on their local machines.
- Set up their personal development environment using **VS Code**.
- Initialized a Git repository, created a file, made commits, and practiced basic Git commands.
- (Optionally) connected to a remote GitHub repository and pushed their changes.

---

This step-by-step lab will provide hands-on experience with Git and setting up a development environment, giving participants the tools to effectively manage their code in real-world DevOps environments. 
