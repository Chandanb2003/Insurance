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

🔐 GCP Service Account Permissions

The Jenkins pipeline uses Terraform, GKE, Docker, and kubectl.
Assign the following IAM roles to the service account used in the pipeline:
roles/container.admin
roles/compute.admin
roles/iam.serviceAccountUser
roles/storage.admin  


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
<img width="1440" height="900" alt="image" src="https://github.com/user-attachments/assets/d779bf22-a80a-45bc-b86e-4a8aeb593793" />
<img width="1440" height="900" alt="image" src="https://github.com/user-attachments/assets/c5a66386-c751-400e-b2c2-f4d5d5a9a325" />
<img width="1440" height="900" alt="image" src="https://github.com/user-attachments/assets/cf76f753-ca63-44a8-8f1c-6d1cdcf6f33b" />
<img width="1440" height="900" alt="image" src="https://github.com/user-attachments/assets/25dae396-7904-49d0-adab-28a2f9ac011f" />
