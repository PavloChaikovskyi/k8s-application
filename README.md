# **Kubernetes App Deployment with Private Image**

## **Project Overview**
This project demonstrates a complete end-to-end deployment process of a 3-tier application using Docker and Kubernetes. The steps include building a Docker image, pushing it to a private Docker registry, testing the application locally with Docker Compose, and finally deploying it to a local Kubernetes cluster using Minikube.

## **Application Architecture**
The application is deployed in a 3-tier architecture consisting of the following layers:

- **Presentation Layer**: Managed by Kubernetes NGINX Ingress for traffic routing.
- **Application Layer**: A Flask web application deployed in Kubernetes pods using a custom Docker image.
- **Data Layer**: A MongoDB database deployed in Kubernetes pods, also using a Docker image.

## **Deployment Steps**

### **1. Build Docker Image and Publish to Private Docker Registry**
   - The application's source code is located in the `/application` directory.
   - The Docker image is built from the source code with no Dockerfile linter errors or warnings.
   - The built image is published to a private Docker registry on Docker Hub.

   ![Docker Hub](img/docker-hub.png)

### **2. Local Deployment using Docker Compose**
   - A `docker-compose.yaml` file is created to deploy the application locally.
   - The application is accessible via a web browser, and data can be written to MongoDB through the **Test DB Connectivity** page.

   ![Docker Compose Application](img/docker-compose-app.png)

### **3. Prepare Kubernetes Manifests and Deploy to Local Minikube Cluster**
   - Kubernetes manifests (`manifest.yml`) are created to deploy the application to a local Minikube Kubernetes cluster.
   - The deployment follows specific requirements for each layer, including configuration of Secrets, ConfigMaps, Deployments, Services, StatefulSets, and Ingress.

   ![Kubernetes Application](img/k8s-deployment.png)

#### **Key Kubernetes Manifest Requirements:**

- **Application Layer**:
  - Deploy using **Deployment** and **Service**.
  - Configure Kubernetes secret for Docker credentials.
  - Set deployment parameters:
    - **Name**: `application`
    - **Replicas**: 1
    - **Ports**: Service port 80, Container port 5000
    - **Probes**: Liveness at `/healthz`, Readiness at `/healthx`
    - **Resources**: CPU limit 0.5, request 0.2; Memory limit 128Mi, request 64Mi
    - Include an init container to wait until MongoDB is available.

- **Data Layer**:
  - Deploy using **StatefulSet**:
    - **Name**: `mongo`
    - **Replicas**: 1
    - Configure resource limits and requests.
    - Use Kubernetes secret for MongoDB credentials.

- **Presentation Layer**:
  - Deploy using **NGINX Ingress**:
    - **Name**: `nginx`
    - **Host**: `pchaikovskyi.application.com`
    - **Port**: 80

### **Deployment Verification**
- Ensure the application is successfully deployed on the local Kubernetes cluster.
- Verify access to the application via `http://pchaikovskyi.application.com`.

   ![Running Application](img/running-app.png)

## **Technologies and Tools Used**
- **Docker**: Docker Desktop, Docker Compose, Docker Hub, Dockerfile
- **Kubernetes**: Secrets, ConfigMaps, Namespaces, Services, Deployments, ReplicaSets, Ingress, StatefulSets, Pods
- **Minikube**: Dashboard, Addons
- **Development Environment**: Visual Studio Code (Terminal)
- **Version Control**: Git, GitHub, GitLab
