# Scalable MERN Microservices Deployment Engine via Jenkins and Kubernetes (Helm)

## 📌 Architectural Overview
This repository contains the production deployment framework for a decoupled, containerized MERN streaming application. The project leverages an automated Git-to-Cloud architecture to optimize development cycles and guarantee application high availability.

### High-Level Engineering Workflow:
1. **Local Workspace Workspace:** Decoupled multi-stage application configurations using specialized container definitions.
2. **Continuous Integration (CI):** Cloud-hosted automation triggers on code commit events to compile and securely push images.
3. **Container Registry Vault:** Highly secure image version management utilizing cloud registries.
4. **Local Cluster Orchestration:** Local container orchestration leveraging virtualized clusters.
5. **Declarative Package Management:** Reusable infrastructure configuration blocks using specialized charting templates.

---

## 🛠️ Tech Stack & Infrastructure Blueprints
* **Frontend Tier:** Multi-stage optimized static client asset delivery interface.
* **Backend Microservices:** Independent runtime environments handling discrete application logic APIs.
* **Automation Server:** Deployed inside an AWS EC2 instance running in background runtime modes.
* **Cloud Infrastructure Registry:** Private container storage hosting secure application builds.
* **Container Orchestration Engine:** Single-node local cluster abstraction layers.
* **Kubernetes Package Manager:** Template engine optimizing declarative manifests.

---

## 🚀 Execution & Command Reference

### 1. Registry Authentication & Image Pipeline Setup
To establish access privileges to your private cloud storage registry and manually verify build operations before full automation, execute:
```powershell
# Authenticate local terminal session with the cloud registry vault
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 259183056581.dkr.ecr.us-east-1.amazonaws.com

# Create specialized repositories for separate application services
aws ecr create-repository --repository-name streaming-frontend --region us-east-1
aws ecr create-repository --repository-name streaming-backend --region us-east-1
```

<img width="940" height="263" alt="image" src="https://github.com/user-attachments/assets/fb1515a1-2090-4d74-b631-92bc50e2e857" />


<img width="940" height="575" alt="image" src="https://github.com/user-attachments/assets/cc5e3eb5-84a1-4c15-ba11-09b9fd9b9e80" />


### 2. Manual Container Construction & Tag Manifests
```powershell
# Compile and tag frontend interface assets
cd D:\Assignments\StreamingApp\frontend
docker build -t streaming-frontend .
docker tag streaming-frontend:latest [YOUR_AWS_ACCOUNT_ID]://
docker push [YOUR_AWS_ACCOUNT_ID]://
```


<img width="940" height="302" alt="image" src="https://github.com/user-attachments/assets/8c599068-c95b-4951-a769-755b9faddfef" />


### 3. Continuous Integration Verification
Every code changes pushed to GitHub automatically invokes the automation engine to execute container compilation routines.

<!-- SCREENSHOT INSERTION: Insert Page 3 (Middle Image - Security Group Inbound Firewall Rules 22 & 8080) here -->
<img width="940" height="140" alt="image" src="https://github.com/user-attachments/assets/e63c1fa1-ea4a-4ad1-a227-a8f0c7493cfd" />

<!-- SCREENSHOT INSERTION: Insert Page 4 (Top/Bottom Images - Green Jenkins Pipeline Stage Matrix View) here -->
<img width="928" height="535" alt="image" src="https://github.com/user-attachments/assets/c09808df-52a7-4d5e-b602-74977224f11a" />


### 4. Kubernetes Local Cluster Boot & Secret Injection
```powershell
# Fire up local single-node cluster node virtualization layer
minikube start --driver=docker

# Inject private registry access passport directly into local cluster namespaces
\$TOKEN = aws ecr get-login-password --region us-east-1
kubectl create secret docker-registry aws-ecr-secret `
  --docker-server=[YOUR_AWS_ACCOUNT_ID].dkr.ecr.us-east-1.amazonaws.com `
  --docker-username=AWS `
  --docker-password=\$TOKEN
```

<!-- SCREENSHOT INSERTION: Insert Page 5 (Top Image - Minikube Status Control Plane Running state) here -->
<img width="940" height="451" alt="image" src="https://github.com/user-attachments/assets/9bf50d82-15de-458b-8a94-fa0ffc803ed5" />


### 5. Declarative Helm Chart Deployment & Tracking
```powershell
# Initialize application release manifests via Helm templates
cd D:\Assignments\StreamingApp
helm install my-streaming-release ./streaming-chart

# Track runtime operational lifecycle status metrics across pods and services
kubectl get pods,svc -w

# Open secure network load-balancing bridge paths directly into browser
minikube service streaming-frontend
```

<!-- SCREENSHOT INSERTION: Insert Page 5 (Bottom Image - Kubectl Get Pods with all 3 entries matching 1/1 Running status rows) here -->
<img width="940" height="146" alt="image" src="https://github.com/user-attachments/assets/7326912b-9a21-466f-93a2-c84d9752584d" />

<!-- SCREENSHOT INSERTION: Insert Page 6 (Top Image - Visual Verification page displaying Welcome to nginx layout interface) here -->
<img width="940" height="235" alt="image" src="https://github.com/user-attachments/assets/a64eccd9-0b5b-407b-ab67-b99795322bed" />

