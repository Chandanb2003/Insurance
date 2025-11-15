🚀 Insurance Application – DevOps Project
📌 Project Overview
This project demonstrates a complete DevOps setup for deploying a Java-based Insurance Application using:

Docker – Containerization
Jenkins – CI/CD pipeline
Terraform – Infrastructure as Code
Ansible – Server configuration
Kubernetes (YAML) – Deployment & service definition

📦 Architecture
Developer → GitHub → Jenkins → Docker Build → Push to Registry
         → Terraform → Provision Infra
         → Ansible → Configure Servers
         → Kubernetes → Deploy Pods/Service

🛠️ Tech Stack
| Tool                | Purpose            |
| ------------------- | ------------------ |
| **Java**            | Application source |
| **Docker**          | Build image        |
| **Jenkins**         | CI/CD              |
| **Terraform**       | Infra provisioning |
| **Ansible**         | Server automation  |
| **Kubernetes YAML** | Deployment         |
| **GitHub**          | Version control    |

⚙️ Project Structure
Insurance/
│── Dockerfile
│── Jenkinsfile
│── ansible-playbook.yml
│── terraform_files/
│── k8s-manifests/
│── src/
│── README.md

🚀 Run Locally (Quickstart)
1️⃣ Build Docker Image
docker build -t insurance-app .

2️⃣ Run Container
docker run -p 8080:8080 insurance-app

📡 CI/CD Pipeline (Jenkins)
The Jenkinsfile contains stages for:
Checkout code
Build Java application
Build & tag Docker image
Push to registry
Deploy using Ansible/K8s

🌐 Deployment on Kubernetes
kubectl apply -f k8s-manifests/

📂 Infrastructure (Terraform)
Terraform files create:
VM/instance
Networking
Security rules

📘 Future Enhancements
Integrate Helm
Add Prometheus + Grafana
Add GitHub Actions pipeline
Add automated testing
