### Explanation of the `fixed-k8s.sh` Script

The provided `fixed-k8s.sh` script is a comprehensive Kubernetes deployment automation script that simplifies the process of deploying a containerized application using Kubernetes. This script has been designed with improvements to avoid common issues encountered in environments like **WSL2 (Windows Subsystem for Linux)** and **AWS EC2**.

* * *

**Key Features of the Script**
------------------------------

1.  **Project Setup**
    
    *   Creates a structured directory layout (`k8s-master-app`) with subfolders for application files, Kubernetes manifests, configuration files, logs, and scripts.
    *   Generates essential Kubernetes YAML configuration files dynamically.
2.  **Application Setup**
    
    *   Defines a **Flask-based Python application (`app.py`)**.
    *   Provides a **Dockerfile** to containerize the application.
    *   Defines the necessary dependencies in `requirements.txt`.
3.  **Kubernetes Resources**
    
    *   **Namespaces:** Defines `k8s-demo` as an isolated environment.
    *   **ConfigMaps & Secrets:** Manages application configuration and sensitive credentials.
    *   **Deployments:** Manages replicas of the application pods.
    *   **Services:** Exposes the application via NodePort for external access.
    *   **Auto-scaling (HPA):** Enables automatic pod scaling based on CPU/memory usage.
4.  **Deployment and Cleanup Automation**
    
    *   **Deployment Script (`deploy.sh`):** Automates the entire Kubernetes setup, including Minikube configuration, Docker build, and resource deployment.
    *   **Cleanup Script (`cleanup.sh`):** Deletes all Kubernetes resources when they are no longer needed.
    *   **Testing Script (`test-app.sh`):** Runs basic health checks on the deployed application.

* * *

**Running the Script on AWS EC2**
---------------------------------

I attempted to run the **`fixed-k8s.sh`** script on an AWS EC2 instance, but encountered several challenges:

### **1\. Minikube Installation Issues**

*   Since **AWS EC2 is a remote environment**, Minikube was not the ideal choice for Kubernetes orchestration.
*   Minikube is best suited for local testing, while AWS uses **Amazon EKS (Elastic Kubernetes Service)** for production Kubernetes clusters.
*   Running Minikube required enabling the **Docker driver**, but EC2 instances had limited CPU cores (less than 2), leading to failure.

**Error Message:**

arduino

`X Exiting due to RSRC_INSUFFICIENT_CORES: Docker has less than 2 CPUs available, but Kubernetes requires at least 2 to be available.` 

**Solution:**

*   Instead of using Minikube, I needed to deploy the application to an AWS EKS cluster.

### **2\. Disk Space Limitations**

*   AWS EC2 instances typically have a **default disk size of 8GB**, which quickly filled up when deploying Kubernetes.
*   Minikube requires more storage for **container images, logs, and persistent volumes**.
*   Expanding storage in AWS required:
    *   Stopping the instance.
    *   Modifying the root volume size in AWS Console.
    *   Running the following command to resize the file system:
        
        `sudo growpart /dev/xvda 1
        sudo resize2fs /dev/xvda1` 
        

### **3\. Kubernetes Cluster Connection Issues**

*   The script relied on Minikube’s built-in Kubernetes API (`192.168.49.2:8443`).
*   In AWS, we needed to use **Amazon EKS**, which required updating the `kubectl` context with:
    
    `aws eks update-kubeconfig --name my-cluster --region us-east-1` 
    
*   The connection was failing due to **incorrect cluster configuration**.

**Error Message:**

pgsql

`The connection to the server 192.168.49.2:8443 was refused - did you specify the right host or port?` 

**Solution:**

*   Instead of using `minikube`, deploy the application to an existing EKS cluster.

### **4\. Port Forwarding Issues**

*   The script tried to expose the application using **port forwarding (`kubectl port-forward`)**, which was not working on AWS EC2.
*   Instead, I needed to expose the application via **LoadBalancer** or **Ingress Controller**.

**Alternative Approach:**

*   Use a **LoadBalancer Service**:
    
    `apiVersion: v1
    kind: Service
    metadata:
      name: k8s-master-app
      namespace: k8s-demo
    spec:
      type: LoadBalancer
      selector:
        app: k8s-master
      ports:
        - protocol: TCP
          port: 80
          targetPort: 5000` 
    

* * *

**Lessons Learned**
-------------------

*   **Minikube is not suited for AWS EC2**; instead, we should use **EKS (Elastic Kubernetes Service)**.
*   **Disk space issues on EC2 instances** can be resolved by expanding the root volume.
*   **Kubernetes API connection issues** arise due to incorrect cluster contexts (`kubectl` should be configured for EKS).
*   **Use LoadBalancer services** instead of port forwarding for better application accessibility in AWS.

* * *

**Next Steps**
--------------

*   Modify the script to **deploy directly to an AWS EKS cluster** instead of Minikube.
*   Update **storage management** to avoid `No Space Left` errors in EC2.
*   Use **AWS Load Balancer** instead of Minikube NodePort services.

* * *
