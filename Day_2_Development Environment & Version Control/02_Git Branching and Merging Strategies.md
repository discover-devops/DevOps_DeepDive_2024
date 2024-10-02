### **Day 2: Development Environment & Version Control**

#### **Hour 2: Git Branching and Merging Strategies**

---

#### **Concepts: Git Workflows, Branching Strategies, Merging Techniques**

Branching and merging are essential parts of any **Git workflow**, allowing multiple developers to work on the same project simultaneously without interfering with each other's changes. In this session, we will explore different **branching strategies**, understand common **merging techniques**, and practice resolving conflicts that can arise during the merge process.

---

#### **Git Workflows**

A **Git workflow** defines how teams use Git to collaborate effectively. There are several popular Git workflows, depending on the complexity and size of the project:

1. **Feature Branch Workflow**:
   - Each new feature or task is developed in its own branch.
   - Once completed, the feature branch is merged back into the main branch.
   - This ensures that the main branch remains stable and that features are developed independently.

2. **GitFlow**:
   - A more structured workflow that includes multiple branches, such as:
     - **Master branch**: Always holds production-ready code.
     - **Develop branch**: Used for integrating features and preparing them for release.
     - **Feature branches**: Where individual features are developed.
     - **Hotfix branches**: For quickly fixing bugs in production.

3. **Forking Workflow**:
   - Common in open-source projects.
   - Each contributor creates a personal fork of the repository and makes changes in their fork.
   - Once the changes are ready, a **pull request** is opened to merge changes back into the main repository.

---

#### **Branching Strategies**

1. **Master/Develop Branching**:
   - The **master branch** contains stable code that is ready for production.
   - The **develop branch** serves as an integration branch for features that are ready to be merged and tested together.

2. **Feature Branching**:
   - Developers create a new branch for each feature or bug fix. Once the feature is completed, it is merged into the **develop** branch or the **master** branch (depending on the workflow).

3. **Hotfix Branching**:
   - A temporary branch created to address urgent bugs or issues in the production environment.
   - After fixing the issue, the branch is merged into both **master** and **develop** to keep the codebase consistent.

---

#### **Merging Techniques**

There are different techniques to merge code back into the main branch. Each technique has its pros and cons, and you can choose one depending on your workflow:

1. **Fast-Forward Merge**:
   - Occurs when the branch being merged is directly ahead of the target branch.
   - Simply moves the pointer forward, without creating a merge commit.
   - Example command:
     ```bash
     git merge --ff feature-branch
     ```

2. **Merge Commit**:
   - A commit that ties together two branches.
   - Useful for keeping the history of both branches.
   - Example command:
     ```bash
     git merge feature-branch
     ```

3. **Rebase**:
   - Moves or combines a series of commits to the tip of another branch.
   - Rebase results in a cleaner Git history, but you lose the context of the original branch.
   - Example command:
     ```bash
     git rebase develop
     ```

4. **Squash and Merge**:
   - Squashes all commits from a feature branch into a single commit before merging.
   - This results in a cleaner history with fewer commits but makes it harder to trace specific changes.
   - Example in GitHub or GitLab merge request options.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Feature Branching in a Continuous Deployment Environment**:
   - Imagine an e-commerce website where new features are constantly being developed. Using **feature branches**, each new feature (e.g., a new payment gateway or product recommendation system) is developed independently. Once fully tested, it is merged into the **develop branch** for integration testing before being pushed to production.

2. **Hotfix Branching in Emergency Scenarios**:
   - Suppose a critical bug in the checkout process is discovered in production. A **hotfix branch** is quickly created from the **master branch**, the bug is fixed, and the branch is merged back into **master** and **develop** to ensure both production and future development are up-to-date.

---

#### **Hands-on Lab: Practice Branching, Merging, and Resolving Conflicts**

##### **Lab Setup**:

Participants should have their **Git** environment set up from the previous session.

---

**Step 1: Create a New Repository or Clone an Existing One**

1. Create a new repository or clone an existing one:
   ```bash
   git clone https://github.com/yourusername/sample-project.git
   cd sample-project
   ```

---

**Step 2: Create and Switch to a New Feature Branch**

1. Create a new branch called `feature-1`:
   ```bash
   git checkout -b feature-1
   ```

2. Make some changes to a file, e.g., `README.md`:
   ```bash
   echo "This is Feature 1" >> README.md
   ```

3. Stage and commit the changes:
   ```bash
   git add README.md
   git commit -m "Add Feature 1"
   ```

---

**Step 3: Create Another Feature Branch**

1. Switch back to the master branch:
   ```bash
   git checkout master
   ```

2. Create a second branch called `feature-2`:
   ```bash
   git checkout -b feature-2
   ```

3. Make different changes to the same `README.md` file:
   ```bash
   echo "This is Feature 2" >> README.md
   ```

4. Stage and commit the changes:
   ```bash
   git add README.md
   git commit -m "Add Feature 2"
   ```

---

**Step 4: Merging Feature Branches and Resolving Conflicts**

1. Switch back to the **master** branch:
   ```bash
   git checkout master
   ```

2. Merge **feature-1** into **master**:
   ```bash
   git merge feature-1
   ```

3. Now, try merging **feature-2** into **master**:
   ```bash
   git merge feature-2
   ```

   At this point, Git will detect a **merge conflict** because both feature branches modified the same lines in the `README.md` file.

4. **Resolve the conflict**:
   - Open the `README.md` file. You will see conflict markers indicating where the two changes overlap:
     ```text
     <<<<<<< HEAD
     This is Feature 1
     =======
     This is Feature 2
     >>>>>>> feature-2
     ```

   - Edit the file to keep the desired content. For example:
     ```text
     This is Feature 1 and Feature 2
     ```

5. After resolving the conflict, stage and commit the merge:
   ```bash
   git add README.md
   git commit -m "Resolve conflict between feature-1 and feature-2"
   ```

---

**Step 5: Push Changes to Remote Repository (Optional)**

1. If using a remote repository (like GitHub or GitLab), push the changes:
   ```bash
   git push origin master
   ```

---

#### **Lab Wrap-Up**

By the end of this lab, participants will have:
- Created and managed feature branches using Git.
- Practiced merging branches into the master branch.
- Resolved merge conflicts manually.
- Understood the importance of using proper branching strategies and workflows in real-world DevOps environments.

---

This lab gives participants practical experience with the most common Git operations, including branching, merging, and conflict resolution, preparing them to manage code collaboration effectively in real-world projects. 
