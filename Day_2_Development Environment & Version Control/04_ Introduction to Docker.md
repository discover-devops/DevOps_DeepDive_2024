### **Day 2: Development Environment & Version Control**

#### **Hour 4: Introduction to Docker**

---

#### **Concepts: Containerization Benefits, Docker Architecture, Dockerfile Basics**

---

**Containerization Benefits:**

Containerization offers a modern approach to application deployment by bundling the code and its dependencies into isolated environments called **containers**. Containers are lightweight, portable, and ensure consistency across different environments.

Key benefits include:
- **Portability**: A containerized application can run the same way across various environments—development, testing, and production—without compatibility issues.
- **Resource Efficiency**: Containers use less system resources than virtual machines since they share the host's operating system kernel.
- **Isolation**: Each container operates independently, ensuring that the failure of one container doesn’t affect others.
- **Scalability**: Containers can be easily scaled up or down depending on demand, making them ideal for microservices architectures.

---

**Docker Architecture:**

Docker is the most popular tool for containerization. The key components of Docker's architecture are:

1. **Docker Engine**: The core of Docker, consisting of:
   - **Docker Daemon**: Runs on the host machine and manages containers, images, and networks.
   - **Docker CLI**: The command-line interface used by developers to interact with Docker.
   
2. **Images**: A Docker image is a lightweight, standalone, and executable package that contains the application and its dependencies. It is the blueprint for a container.

3. **Containers**: A container is a running instance of a Docker image. Containers run isolated processes in their own environments but share the host’s kernel.

4. **Dockerfile**: A text file that defines the steps to create a Docker image. It contains instructions for building the image (e.g., what base image to use, what application to run).

5. **Docker Hub**: A public registry where Docker images can be stored, shared, and pulled from.

---

**Dockerfile Basics:**

A **Dockerfile** is the blueprint for creating Docker images. It contains a series of instructions that define how to build an image.

Key Dockerfile instructions include:
- **FROM**: Defines the base image. Every Dockerfile starts with a base image (e.g., Ubuntu, Node.js, Python).
- **COPY**: Copies files from your local filesystem to the image.
- **RUN**: Executes commands in the image (e.g., installing packages).
- **CMD**: Specifies the command to run when the container starts.
- **EXPOSE**: Specifies the port on which the container will listen.

**Example Dockerfile**:
```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Run the app when the container launches
CMD ["python", "app.py"]
```

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Portability in Multi-Cloud Environments**:
   - In a multi-cloud architecture where an application is deployed across AWS, Azure, and Google Cloud, containerization ensures the application runs in a consistent environment across all cloud providers. Docker containers eliminate compatibility issues by bundling the application with its dependencies.

2. **Real-World Use Case: Scaling Microservices**:
   - An e-commerce platform with different services (e.g., payment, catalog, and user management) can containerize each microservice. Containers allow developers to scale each service independently based on demand. For instance, the payment service can be scaled during high traffic, without affecting other services.

---

#### **Hands-on Lab: Build and Run a Docker Container from a Simple Dockerfile**

##### **Lab Setup**:

Participants should have **Docker** installed on their local machine. You can download Docker from the [official Docker website](https://www.docker.com/).

---

#### **Lab 1: Building a Simple Docker Image for a Python Application**

In this lab, participants will create a simple Python application, write a Dockerfile for it, and build and run the Docker container.

---

**Step 1: Create a Simple Python Application**

1. Create a new directory for your project:
   ```bash
   mkdir my-python-app && cd my-python-app
   ```

2. Inside the directory, create a file named `app.py` with the following content:
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello, Docker!'

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=80)
   ```

3. Create a `requirements.txt` file for Python dependencies:
   ```text
   Flask
   ```

---

**Step 2: Write a Dockerfile**

1. Inside the `my-python-app` directory, create a Dockerfile:
   ```bash
   touch Dockerfile
   ```

2. Open the Dockerfile in an editor and add the following content:
   ```dockerfile
   # Use the official Python image from Docker Hub
   FROM python:3.8-slim

   # Set the working directory in the container
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY . /app

   # Install the dependencies specified in requirements.txt
   RUN pip install -r requirements.txt

   # Expose port 80 for the web service
   EXPOSE 80

   # Run the application when the container launches
   CMD ["python", "app.py"]
   ```

---

**Step 3: Build the Docker Image**

1. In the terminal, navigate to your project directory and run the following command to build the Docker image:
   ```bash
   docker build -t my-python-app .
   ```

   - This command builds the Docker image using the Dockerfile and tags it as `my-python-app`.

2. Verify the image is built:
   ```bash
   docker images
   ```

---

**Step 4: Run the Docker Container**

1. Run the container:
   ```bash
   docker run -p 4000:80 my-python-app
   ```

   - This maps port 4000 on your local machine to port 80 in the container.

2. Open a web browser and go to `http://localhost:4000`. You should see the message "Hello, Docker!"

3. Stop the container by pressing **Ctrl+C** in the terminal.

---

#### **Lab 2: Modifying the Dockerfile for a Node.js Application**

**Scenario**: Participants will modify the Dockerfile to build and run a Node.js application.

---

**Step 1: Create a Simple Node.js Application**

1. Create a new directory for your project:
   ```bash
   mkdir my-node-app && cd my-node-app
   ```

2. Inside the directory, create a `server.js` file with the following content:
   ```javascript
   const express = require('express');
   const app = express();

   app.get('/', (req, res) => {
     res.send('Hello, Docker with Node.js!');
   });

   app.listen(80, () => {
     console.log('Server is running on port 80');
   });
   ```

3. Create a `package.json` file:
   ```json
   {
     "name": "my-node-app",
     "version": "1.0.0",
     "description": "A simple Node.js app running in Docker",
     "main": "server.js",
     "dependencies": {
       "express": "^4.17.1"
     }
   }
   ```

---

**Step 2: Write a Dockerfile**

1. Inside the `my-node-app` directory, create a Dockerfile:
   ```bash
   touch Dockerfile
   ```

2. Add the following content to the Dockerfile:
   ```dockerfile
   # Use the official Node.js image from Docker Hub
   FROM node:14

   # Set the working directory
   WORKDIR /usr/src/app

   # Copy package.json and install dependencies
   COPY package.json ./
   RUN npm install

   # Copy the rest of the application code
   COPY . .

   # Expose port 80
   EXPOSE 80

   # Run the application
   CMD ["node", "server.js"]
   ```

---

**Step 3: Build and Run the Docker Container**

1. Build the Docker image:
   ```bash
   docker build -t my-node-app .
   ```

2. Run the Docker container:
   ```bash
   docker run -p 4001:80 my-node-app
   ```

3. Open `http://localhost:4001` in a browser to see "Hello, Docker with Node.js!"

---

#### **Lab 3: Running Multiple Containers with Docker Compose**

**Scenario**: Participants will run multiple containers (Python and Node.js) using **Docker Compose**.

---

**Step 1: Create a `docker-compose.yml` File**

1. In the root directory of your project, create a `docker-compose.yml` file:
   ```bash
   touch docker-compose.yml
   ```

2. Add the following content:
   ```yaml
   version: '3'
   services:
     python-app:
       build: ./my-python-app
       ports:
         - "5000:80"
     node-app:
       build: ./my-node-app
      

 ports:
         - "5001:80"
   ```

---

**Step 2: Build and Run the Services**

1. Run Docker Compose to build and start both services:
   ```bash
   docker-compose up
   ```

2. Access the applications:
   - Python app: `http://localhost:5000`
   - Node.js app: `http://localhost:5001`

3. To stop the services:
   ```bash
   docker-compose down
   ```

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Built and run Docker containers for Python and Node.js applications.
- Modified a Dockerfile for different application types.
- Used Docker Compose to manage multiple containers in a single environment.

---

These labs provide practical experience with Docker, from basic container creation to advanced setups with Docker Compose. 
