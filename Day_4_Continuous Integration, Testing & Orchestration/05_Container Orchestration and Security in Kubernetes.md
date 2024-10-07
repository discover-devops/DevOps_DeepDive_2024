### **Day 4: Continuous Integration, Testing & Orchestration**

#### **Hour 5: Container Orchestration and Security in Kubernetes**

---

#### **Concepts: Security Best Practices in Kubernetes**

As containerized applications are deployed at scale in Kubernetes clusters, securing these workloads becomes crucial. Kubernetes offers various built-in security features to protect your cluster, workloads, and communication between Pods.

---

**Key Kubernetes Security Concepts**:

1. **Role-Based Access Control (RBAC)**:
   - RBAC is used to control access to Kubernetes resources based on roles and permissions. By defining **Roles**, **RoleBindings**, **ClusterRoles**, and **ClusterRoleBindings**, you can restrict what users and services can do within the cluster.

2. **Network Policies**:
   - Network policies define how Pods are allowed to communicate with each other and other network endpoints. By implementing network policies, you can ensure that only authorized services can talk to each other, reducing the attack surface.

3. **Pod Security Policies (PSPs)**:
   - PSPs provide fine-grained control over how Pods are allowed to operate, including restrictions on running privileged containers, using specific volume types, or enabling host namespaces.

4. **Secrets Management**:
   - Kubernetes allows for managing sensitive information like passwords, API tokens, and certificates via **Secrets**, which can be securely injected into Pods.

5. **Image Security**:
   - It’s important to ensure that only trusted and signed container images are used. By configuring **image policies** and using image scanning tools, you can detect vulnerabilities before they make it to production.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Securing Access to Sensitive Resources**:
   - In a company running multiple teams with different levels of access, **RBAC** is used to ensure that only the DevOps team can modify Kubernetes Deployments, while the QA team can only view logs and check the status of Pods.
   
2. **Real-World Use Case: Restricting Network Communication in Microservices**:
   - A microservice architecture has multiple services, some of which handle sensitive data. The team uses **Network Policies** to restrict access so that only specific services (e.g., the frontend) can communicate with the backend services handling sensitive data.

---

#### **Hands-on Lab: Explore Kubernetes Security Features (RBAC, Network Policies)**

In this lab, participants will secure a Kubernetes cluster by configuring **RBAC** for access control and defining **Network Policies** to control communication between Pods.

---

### **Lab 1: Implementing Role-Based Access Control (RBAC) in Kubernetes**

**Prerequisites**:
- A working Kubernetes cluster (Minikube or a cloud provider).
- `kubectl` command-line tool.

---

**Step 1: Create a Service Account for Limited Access**

1. **Create a Service Account**:
   - Service accounts are used to provide different permissions to services running inside the cluster. Create a service account with limited access:
     ```bash
     kubectl create serviceaccount devops-sa
     ```

2. **Create a Role with Specific Permissions**:
   - A **Role** grants specific permissions within a namespace. Create a YAML file (`role.yml`) to define a role that allows read-only access to Pods:
     ```yaml
     apiVersion: rbac.authorization.k8s.io/v1
     kind: Role
     metadata:
       namespace: default
       name: pod-reader
     rules:
     - apiGroups: [""]  # "" indicates the core API group
       resources: ["pods"]
       verbs: ["get", "list"]
     ```

3. **Create a RoleBinding to Bind the Role to the Service Account**:
   - Create a **RoleBinding** to bind the role to the `devops-sa` service account:
     ```yaml
     apiVersion: rbac.authorization.k8s.io/v1
     kind: RoleBinding
     metadata:
       name: read-pods-binding
       namespace: default
     subjects:
     - kind: ServiceAccount
       name: devops-sa
       namespace: default
     roleRef:
       kind: Role
       name: pod-reader
       apiGroup: rbac.authorization.k8s.io
     ```

4. **Apply the Role and RoleBinding**:
   ```bash
   kubectl apply -f role.yml
   kubectl apply -f rolebinding.yml
   ```

---

**Step 2: Test RBAC Permissions**

1. **Switch to the Service Account**:
   - Use the service account to test permissions:
     ```bash
     kubectl auth can-i get pods --as=system:serviceaccount:default:devops-sa
     ```

2. **Test Unauthorized Access**:
   - Try to perform a forbidden action (e.g., delete a Pod) and verify that it is denied:
     ```bash
     kubectl auth can-i delete pods --as=system:serviceaccount:default:devops-sa
     ```

---

### **Lab 2: Implementing Network Policies in Kubernetes**

**Step 1: Define a Network Policy**

1. **Create a Network Policy**:
   - Network policies are used to control which Pods can communicate with each other. Create a YAML file (`network-policy.yml`) that restricts communication so that only Pods with a specific label can access the app:
     ```yaml
     apiVersion: networking.k8s.io/v1
     kind: NetworkPolicy
     metadata:
       name: allow-frontend
       namespace: default
     spec:
       podSelector:
         matchLabels:
           app: backend
       policyTypes:
       - Ingress
       ingress:
       - from:
         - podSelector:
             matchLabels:
               role: frontend
         ports:
         - protocol: TCP
           port: 5000
     ```

2. **Apply the Network Policy**:
   - Apply the network policy to the cluster:
     ```bash
     kubectl apply -f network-policy.yml
     ```

---

**Step 2: Test Network Policy Restrictions**

1. **Test Pod Communication**:
   - Deploy two Pods, one with the label `frontend` and one with `backend`.
   - Verify that the `frontend` Pod can communicate with the `backend` Pod:
     ```bash
     kubectl run frontend --image=nginx --labels="role=frontend"
     kubectl run backend --image=nginx --labels="app=backend"
     ```

2. **Test Unauthorized Access**:
   - Try to access the `backend` service from another Pod that doesn't have the `frontend` label and verify that communication is blocked.

---

### **Lab Wrap-Up**

By the end of these labs, participants will have:
- Implemented **RBAC** to control access to Kubernetes resources.
- Created and applied **Network Policies** to control communication between Pods.
- Tested secure access controls and communication in a Kubernetes environment.

---

These labs provide hands-on experience with Kubernetes security features, helping participants understand how to protect their cluster from unauthorized access and control network traffic between services. 
