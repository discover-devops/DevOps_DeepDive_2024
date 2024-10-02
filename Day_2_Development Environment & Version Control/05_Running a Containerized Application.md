### **Day 2: Development Environment & Version Control**

#### **Hour 5: Running a Containerized Application**

---

#### **Concepts: Docker Commands, Managing Containers, Port Mapping**

In this session, we will focus on learning the basic Docker commands for running, managing, and troubleshooting containers, with an emphasis on understanding port mapping for exposing applications to external users.

---

**Key Docker Commands:**

1. **Running Containers**:
   - The most fundamental Docker operation is running a container from an image.
   - Command:
     ```bash
     docker run [options] IMAGE [command]
     ```

2. **Listing Running and Stopped Containers**:
   - View all currently running containers:
     ```bash
     docker ps
     ```
   - View all containers (running and stopped):
     ```bash
     docker ps -a
     ```

3. **Starting and Stopping Containers**:
   - Start a stopped container:
     ```bash
     docker start CONTAINER_ID
     ```
   - Stop a running container:
     ```bash
     docker stop CONTAINER_ID
     ```

4. **Removing Containers**:
   - To remove a container (make sure it is stopped first):
     ```bash
     docker rm CONTAINER_ID
     ```

5. **Viewing Logs**:
   - View the logs of a container:
     ```bash
     docker logs CONTAINER_ID
     ```

6. **Executing Commands Inside a Running Container**:
   - Run commands interactively in a running container:
     ```bash
     docker exec -it CONTAINER_ID bash
     ```

---

**Port Mapping:**

Containers are isolated environments, but they need to expose certain services to the outside world. **Port mapping** is used to make services running inside a container accessible from the host machine.

- **Port Mapping Syntax**:
  ```bash
  docker run -p HOST_PORT:CONTAINER_PORT IMAGE
  ```
  - Example: To map port 8080 on the host to port 80 in the container:
    ```bash
    docker run -p 8080:80 IMAGE
    ```

- **Exposing Ports in the Dockerfile**:
  - The `EXPOSE` command in the Dockerfile indicates that a container listens on a specific port.
  ```dockerfile
  EXPOSE 80
  ```

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Running Multiple Web Applications**:
   - Imagine a development team working on two web applications: one for a product catalog and one for user authentication. By using port mapping, both applications can run on the same machine but on different ports (e.g., 8080 for the catalog app and 8081 for the authentication app), ensuring they don’t interfere with each other.

2. **Real-World Use Case: Scaling Microservices**:
   - In a microservices architecture, each service can run in its own container and be exposed on different ports. Port mapping allows the services to communicate with each other or with the outside world, while maintaining isolation.

---

#### **Hands-on Lab: Running a Sample Application in a Docker Container**

##### **Lab Setup**:

Participants should have **Docker** installed from the previous sessions. They will run a simple web application inside a Docker container and practice managing it using Docker commands.

---

#### **Lab 1: Running a Simple Web Application in a Container**

**Scenario**: In this lab, participants will run a pre-built Docker image of a simple NGINX web server and expose it to their local machine.

---

**Step 1: Pull the NGINX Docker Image**

1. In your terminal, pull the official **NGINX** Docker image from Docker Hub:
   ```bash
   docker pull nginx
   ```

2. Verify that the image has been pulled by listing your Docker images:
   ```bash
   docker images
   ```

---

**Step 2: Run the NGINX Container with Port Mapping**

1. Run the NGINX container with port mapping. In this case, map port 8080 on the host machine to port 80 in the container:
   ```bash
   docker run -d -p 8080:80 --name my-nginx nginx
   ```

   - `-d`: Runs the container in detached mode (in the background).
   - `-p 8080:80`: Maps port 8080 on your local machine to port 80 inside the container.
   - `--name my-nginx`: Assigns the name `my-nginx` to the container for easier management.

2. Open a web browser and go to `http://localhost:8080`. You should see the default NGINX welcome page.

---

**Step 3: Managing the NGINX Container**

1. List the running containers to verify that the NGINX container is active:
   ```bash
   docker ps
   ```

2. View the logs for the container to check the activity:
   ```bash
   docker logs my-nginx
   ```

3. Access the running container and explore the NGINX server’s filesystem:
   ```bash
   docker exec -it my-nginx bash
   ```

4. Stop the running container:
   ```bash
   docker stop my-nginx
   ```

5. Start the container again:
   ```bash
   docker start my-nginx
   ```

6. Remove the container when you’re done:
   ```bash
   docker rm -f my-nginx
   ```

---

#### **Lab 2: Building and Running a Custom Containerized Application**

**Scenario**: In this lab, participants will write a Dockerfile for a basic Node.js web application, build an image from the Dockerfile, and run the container.

---

**Step 1: Create a Simple Node.js Application**

1. Create a new project folder for the Node.js app:
   ```bash
   mkdir my-node-app && cd my-node-app
   ```

2. Create a file named `server.js` with the following content:
   ```javascript
   const http = require('http');
   const server = http.createServer((req, res) => {
     res.statusCode = 200;
     res.setHeader('Content-Type', 'text/plain');
     res.end('Hello from Docker!\n');
   });
   server.listen(80, () => {
     console.log('Server running on port 80');
   });
   ```

3. Create a `package.json` file:
   ```json
   {
     "name": "my-node-app",
     "version": "1.0.0",
     "description": "A simple Node.js web app running in Docker",
     "main": "server.js",
     "dependencies": {
       "http": "^0.0.1"
     }
   }
   ```

---

**Step 2: Write a Dockerfile**

1. In the same directory, create a `Dockerfile`:
   ```bash
   touch Dockerfile
   ```

2. Add the following content to the Dockerfile:
   ```dockerfile
   # Use the official Node.js image
   FROM node:14

   # Set the working directory in the container
   WORKDIR /app

   # Copy package.json and install dependencies
   COPY package.json /app/
   RUN npm install

   # Copy the rest of the application
   COPY . /app

   # Expose port 80
   EXPOSE 80

   # Run the application
   CMD ["node", "server.js"]
   ```

---

**Step 3: Build and Run the Docker Container**

1. Build the Docker image from the Dockerfile:
   ```bash
   docker build -t my-node-app .
   ```

2. Run the Docker container and map port 8081 on your machine to port 80 in the container:
   ```bash
   docker run -d -p 8081:80 my-node-app
   ```

3. Open `http://localhost:8081` in your browser, and you should see "Hello from Docker!".

---

**Step 4: Manage the Node.js Container**

1. List the running containers:
   ```bash
   docker ps
   ```

2. View the logs:
   ```bash
   docker logs CONTAINER_ID
   ```

3. Stop and remove the container:
   ```bash
   docker stop CONTAINER_ID
   docker rm CONTAINER_ID
   ```

---

#### **Lab 3: Running Multiple Containers Using Docker Compose**

**Scenario**: Participants will run both the NGINX and Node.js containers simultaneously using **Docker Compose**.

---

**Step 1: Write a `docker-compose.yml` File**

1. In the root directory, create a `docker-compose.yml` file:
   ```bash
   touch docker-compose.yml
   ```

2. Add the following content to run both the NGINX and Node.js containers:
   ```yaml
   version: '3'
   services:
     nginx:
       image: nginx
       ports:
         - "8080:80"
     node:
       build: ./my-node-app
       ports:
         - "8081:80"
   ```

---

**Step 2: Build and Run the Containers**

1. Run Docker Compose to build and start both services:
   ```bash
   docker-compose up
   ```

2. Verify that both the NGINX and Node.js applications are accessible:
   - NGINX: `http://localhost:8080`
   - Node.js: `http://localhost:8081`

3. To stop and remove the containers:
   ```bash
   docker-compose down
   ```

---

#### **Lab Wrap-Up**

By the end of these labs, participants will have

:
- Run Docker containers using basic Docker commands.
- Managed containers by starting, stopping, viewing logs, and interacting with running containers.
- Set up port mappings to expose containerized applications.
- Built and ran custom containerized applications using Docker.

---

These labs give participants hands-on experience with running and managing containers, setting them up for real-world usage in Docker environments. 
