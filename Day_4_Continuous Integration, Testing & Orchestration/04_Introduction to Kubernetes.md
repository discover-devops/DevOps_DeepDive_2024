### **Day 4: Continuous Integration, Testing & Orchestration**

#### **Hour 4: Introduction to Kubernetes**

---

#### **Concepts: Kubernetes Fundamentals**

**Kubernetes** is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It has become the de facto standard for running modern, cloud-native applications at scale.

---

**Key Kubernetes Concepts**:

1. **Pods**:
   - A **Pod** is the smallest, most basic deployable object in Kubernetes. It represents a single instance of a running process in your cluster, and it typically contains one or more containers that share the same network namespace.
   - Pods are ephemeral, meaning they can be created, destroyed, or rescheduled by Kubernetes to ensure availability and scalability.

2. **Deployments**:
   - A **Deployment** is a higher-level object that defines how many replicas of a Pod should run, how to update those Pods, and how to ensure they are always running correctly.
   - Deployments provide a declarative way to manage applications, allowing Kubernetes to handle rolling updates, rollbacks, and scaling.

3. **Services**:
   - A **Service** in Kubernetes is an abstraction that defines a logical set of Pods and provides a stable endpoint for accessing them.
   - Services allow Pods to communicate with each other, and they can expose applications to the outside world.

---

**Explaining the Concept Using Real-World Use Cases**:

1. **Real-World Use Case: Deploying a Microservice Application**:
   - A company deploying a microservice architecture uses Kubernetes to manage the lifecycle of individual microservices. Each microservice runs inside its own Pod, and **Deployments** ensure the correct number of instances are running at all times. Kubernetes **Services** are used to expose these microservices to the rest of the system and allow communication between them.

2. **Real-World Use Case: Handling Application Failures**:
   - A web application running in a Kubernetes cluster is configured with a **Deployment** that specifies multiple replicas of the web service. If a Pod crashes, Kubernetes automatically spins up a new Pod to replace the failed instance, ensuring the application remains available to users.

---

#### **Hands-on Lab: Deploy a Containerized App on a Local Kubernetes Cluster**

In this lab, participants will set up a local Kubernetes cluster using **Minikube** (a tool that runs a single-node Kubernetes cluster locally). They will deploy a simple containerized web application and expose it using a Kubernetes Service.

---

### **Lab 1: Setting Up Kubernetes Locally with Minikube**

**Prerequisites**:
- **Minikube** and **kubectl** (the Kubernetes CLI) installed on your local machine.
- **Docker** installed to build and run containers.

---

**Step 1: Install Minikube and kubectl**

1. **Install Minikube**:
   - On Linux:
     ```bash
     curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
     sudo install minikube-linux-amd64 /usr/local/bin/minikube
     ```
   - On macOS:
     ```bash
     brew install minikube
     ```

2. **Install kubectl**:
   - On Linux:
     ```bash
     sudo apt-get install -y kubectl
     ```
   - On macOS:
     ```bash
     brew install kubectl
     ```

---

**Step 2: Start Minikube**

1. Start a local Kubernetes cluster using Minikube:
   ```bash
   minikube start
   ```

2. Verify that Minikube is running:
   ```bash
   kubectl get nodes
   ```

---

**Step 3: Create a Simple Docker Image for the Web Application**

1. **Create a simple web app** (`app.py`):
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello, Kubernetes!'

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5000)
   ```

2. **Create a Dockerfile** to containerize the app:
   ```dockerfile
   FROM python:3.8-slim

   WORKDIR /app

   COPY app.py /app

   RUN pip install flask

   CMD ["python", "app.py"]
   ```

3. **Build the Docker image**:
   - Ensure that Docker is running and build the image locally:
     ```bash
     docker build -t my-flask-app .
     ```

4. **Verify the Docker image**:
   ```bash
   docker images
   ```

---

**Step 4: Deploy the Containerized Application on Kubernetes**

1. **Create a Kubernetes Deployment**:
   - Create a file named `deployment.yml` to define a Deployment that runs the containerized app:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: flask-app-deployment
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: flask-app
       template:
         metadata:
           labels:
             app: flask-app
         spec:
           containers:
           - name: flask-app
             image: my-flask-app:latest
             ports:
             - containerPort: 5000
     ```

2. **Apply the Deployment**:
   - Apply the Deployment to your Kubernetes cluster:
     ```bash
     kubectl apply -f deployment.yml
     ```

3. **Verify the Deployment**:
   - Check the status of the Pods:
     ```bash
     kubectl get pods
     ```

---

**Step 5: Expose the Application Using a Kubernetes Service**

1. **Create a Service**:
   - Create a file named `service.yml` to define a Service that exposes the app to the outside world:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: flask-app-service
     spec:
       selector:
         app: flask-app
       ports:
         - protocol: TCP
           port: 80
           targetPort: 5000
       type: LoadBalancer
     ```

2. **Apply the Service**:
   - Apply the Service configuration to the Kubernetes cluster:
     ```bash
     kubectl apply -f service.yml
     ```

3. **Access the Application**:
   - Get the **Minikube service URL**:
     ```bash
     minikube service flask-app-service --url
     ```
   - Open the URL in your web browser, and you should see the message: "Hello, Kubernetes!"

---

### **Lab Wrap-Up**

By the end of this lab, participants will have:
- Deployed a simple containerized web application on a local Kubernetes cluster using **Minikube**.
- Used **Kubernetes Deployments** to ensure the application is running with multiple replicas.
- Exposed the application to the outside world using a **Kubernetes Service**.

---

This lab provides hands-on experience with deploying and managing applications on Kubernetes, giving participants a strong foundation in using Kubernetes for container orchestration.
