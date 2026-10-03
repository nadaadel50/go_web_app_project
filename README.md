# `README.md`

````markdown
# Go Web App — DevOps CI/CD Project

A production-oriented DevOps project demonstrating how to containerize, continuously integrate, and continuously deploy a Go web application to Kubernetes using Docker, Helm, and CI/CD automation.

The project focuses on implementing a complete DevOps workflow from source code to a running Kubernetes workload.

---

## 🚀 Project Overview

This project demonstrates an end-to-end DevOps pipeline for a Go web application.

The workflow automates:

- Source code management with Git/GitHub
- Application build and testing
- Static code analysis
- Docker image creation
- Container image publishing
- Helm chart version/image updates
- Kubernetes deployment
- Continuous Delivery

### High-Level Workflow

```text
                   ┌─────────────────┐
                   │     GitHub      │
                   │  Source Code    │
                   └────────┬────────┘
                            │
                            ▼
                   ┌─────────────────┐
                   │       CI        │
                   │ Build & Test    │
                   └────────┬────────┘
                            │
                            ▼
                   ┌─────────────────┐
                   │ Static Analysis │
                   │  Code Quality   │
                   └────────┬────────┘
                            │
                            ▼
                   ┌─────────────────┐
                   │ Docker Build    │
                   │ & Image Push    │
                   └────────┬────────┘
                            │
                            ▼
                   ┌─────────────────┐
                   │   Helm Chart    │
                   │ Image Update    │
                   └────────┬────────┘
                            │
                            ▼
                   ┌─────────────────┐
                   │       CD        │
                   │ Kubernetes      │
                   │    Deployment   │
                   └─────────────────┘
````

---

## 🏗️ Architecture

The application is packaged as a Docker container and deployed to Kubernetes using a Helm chart.

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
CI/CD Pipeline
    │
    ├── Build
    ├── Test
    ├── Static Code Analysis
    ├── Docker Build
    └── Docker Push
            │
            ▼
      Container Registry
            │
            ▼
       Helm Chart
            │
            ▼
        Kubernetes
            │
            ├── Deployment
            ├── Pod
            └── Service
```

---

# 🛠️ Technology Stack

| Category           | Technology                          |
| ------------------ | ----------------------------------- |
| Application        | Go                                  |
| Version Control    | Git / GitHub                        |
| Containerization   | Docker                              |
| Orchestration      | Kubernetes                          |
| Package Management | Helm                                |
| CI/CD              | Github Actions                      |
| Container Registry | Docker Hub                          |
| Local Kubernetes   | Minikube/ EKS                       |
| OS / Environment   | Linux / Ubuntu                      |

---

# 🐳 Docker

The application is containerized using Docker to provide a consistent runtime environment across development, testing, and deployment environments.

### Build the image

```bash
docker build -t go-web-app .
```

### Run the container

```bash
docker run -p 8080:8080 go-web-app
```

The application can then be accessed at:

```text
http://localhost:8080
```

### Push the image

```bash
docker tag go-web-app <registry>/<username>/go-web-app:<tag>

docker push <registry>/<username>/go-web-app:<tag>
```

---

# ☸️ Kubernetes Deployment

Kubernetes is used to orchestrate the application container.

The deployment configuration defines the desired state of the application, including:

* Number of replicas
* Container image
* Container port
* Service configuration
* Resource configuration
* Deployment strategy

### Check the cluster

```bash
kubectl get nodes
```

### Deploy using Kubernetes manifests

```bash
kubectl apply -f k8s/manifests/
```

### Check deployments

```bash
kubectl get deployments
```

### Check pods

```bash
kubectl get pods
```

### Check services

```bash
kubectl get svc
```

---

# ⛵ Helm

Helm is used to package and manage the Kubernetes deployment.

Instead of maintaining hard-coded Kubernetes configurations for every environment, Helm allows deployment parameters to be managed through chart values.

### Install the application

```bash
helm install go-web-app ./helm/go-web-app
```
---

# 🔄 CI/CD Pipeline

The CI/CD pipeline automates the application lifecycle.

## Continuous Integration

When changes are pushed to the repository, the CI pipeline performs the following steps:

### 1. Checkout

The latest source code is retrieved from GitHub.

### 2. Build

The Go application is compiled to verify that the source code builds successfully.

Example:

```bash
go build ./...
```

### 3. Test

Automated tests are executed.

```bash
go test ./...
```

### 4. Static Code Analysis

The source code is analyzed for potential quality issues.

This step helps identify problems before the application is packaged and deployed.

### 5. Docker Build

A Docker image is created from the application.

```bash
docker build -t go-web-app:${IMAGE_TAG} .
```

### 6. Push Image

The image is pushed to the configured container registry.

```text
Container Registry
        │
        │
        ▼
go-web-app:<tag>
```

---

# 🚀 Continuous Delivery

After the Docker image is successfully published, the deployment stage updates the Kubernetes application.

The Helm chart is updated with the new image tag.

Example:

```yaml
image:
  repository: <registry>/<username>/go-web-app
  tag: <new-tag>
```

The updated Helm release is then deployed to Kubernetes.

```bash
helm upgrade --install go-web-app ./helm/go-web-app
```

This allows the deployment process to use the newly built container image without manually modifying Kubernetes manifests.

---

# 🔐 DevOps & Security Practices

The project follows several DevOps and security principles:

* Containerized application
* Infrastructure/application configuration stored as code
* Automated build and test process
* Static code analysis
* Versioned Docker images
* Kubernetes-based deployment
* Helm-based release management
* Separation of application and deployment configuration
* Secrets should be stored in CI/CD secret management rather than committed to Git
* `.gitignore` used to prevent sensitive/local files from being committed

Sensitive information such as:

```text
API keys
Passwords
Cloud credentials
Registry credentials
Kubernetes secrets
CI/CD tokens
```

should never be hard-coded in the repository.

---

# 🧪 Local Development

## Prerequisites

Install the following tools:

* Go
* Git
* Docker
* kubectl
* Kubernetes cluster such as Minikube
* Helm

Verify the installations:

```bash
go version
git --version
docker --version
kubectl version --client
helm version
```

---

## Run the application locally

Clone the repository:

```bash
git clone <repository-url>
```

Move into the project:

```bash
cd go-web-app
```

Run the application:

```bash
go run .
```

---

# ☸️ Running with Minikube

Start Minikube:

```bash
minikube start
```

Verify the cluster:

```bash
kubectl get nodes
```

Deploy the application:

```bash
helm upgrade --install go-web-app ./helm/go-web-app
```

Check the resources:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```

---


# 📊 Deployment Verification

After deployment, verify:

```bash
kubectl get pods
```

Expected result:

```text
NAME                            READY   STATUS    RESTARTS   AGE
go-web-app-xxxxxxxxxx-xxxxx    1/1     Running   0          ...
```

Check the service:

```bash
kubectl get svc
```

Then access the application through the exposed Kubernetes service.

---

# 🔁 Deployment Strategy

The project uses Kubernetes Deployments to manage application updates.

When a new Docker image is released:

```text
New Git Commit
      │
      ▼
CI Pipeline
      │
      ├── Build
      ├── Test
      ├── Code Analysis
      └── Docker Build
             │
             ▼
       Container Registry
             │
             ▼
        New Image Tag
             │
             ▼
        Helm Upgrade
             │
             ▼
        Kubernetes
             │
             ▼
       Updated Pods
```

This approach provides a repeatable and automated deployment process.


# 🎯 What I Learned

Through this project, I practiced:

* Building and testing a Go application
* Creating production-ready Docker images
* Understanding CI/CD pipelines
* Automating application builds
* Container image management
* Kubernetes deployments
* Kubernetes services
* Helm chart development
* Automated Kubernetes releases
* Debugging Kubernetes workloads
* Managing application versions through image tags
* Applying DevOps principles to a software delivery workflow

---

# 👩‍💻 Author

**Nada Adel**

Computer Science Graduate | DevOps / Cloud Engineer

Interested in:

* DevOps
* DevSecOps
* Cloud Engineering
* Kubernetes
* Infrastructure as Code
* CI/CD
* Backend Development


````
 I can turn this into a **100% accurate README for your exact project**.
