🌱 Plant Disease Detection - DevOps CI/CD Project

A complete **DevOps implementation project** for a **Plant Disease Detection Web Application** using:

Frontend + Backend architecture
Docker containerization
Docker Hub image registry
Jenkins CI/CD pipeline
Kubernetes deployment

🚀 Tech Stack

Frontend

React.js
Nginx

Backend

Python Flask

DevOps Tools

Docker
Docker Hub
Jenkins
Kubernetes
GitHub

📂 Project Structure

bash
PD-Devops-Project/
│
├── frontend/
│   ├── Dockerfile
│   └── React Application
│
├── backend/
│   ├── Dockerfile
│   └── Flask Application
│
├── k8s/
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   └── frontend-service.yaml
│
├── Jenkinsfile
│
└── README.md


⚙️ Features

✅ Frontend containerized using Docker
✅ Backend containerized using Docker
✅ Docker images pushed to Docker Hub
✅ Jenkins CI/CD pipeline automation
✅ Kubernetes deployment automation
✅ Automatic image updates using Jenkins
✅ NodePort services for frontend/backend access

🐳 Docker Setup

Backend Docker Build

```bash
cd backend

docker build -t nithinp004/pd-backend:latest .
```

Run backend:

```bash
docker run -d -p 5000:5000 nithinp004/pd-backend:latest
```

Backend URL:

```text
http://localhost:5000
```

---

Frontend Docker Build

```bash
cd frontend

docker build -t nithinp004/pd-frontend:latest .
```

Run frontend:

```bash
docker run -d -p 3000:80 nithinp004/pd-frontend:latest
```

Frontend URL:

```text
http://localhost:3000
```

---

☸️ Kubernetes Setup

Apply Kubernetes manifests:

```bash
kubectl apply -f k8s/
```

Check pods:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get svc
```

---

🌐 Kubernetes Service URLs

Frontend

```text
http://localhost:32546
```

Backend

```text
http://localhost:31379
```

---

🔄 Jenkins CI/CD Pipeline

The Jenkins pipeline automates:

1. Clone GitHub Repository
2. Build Docker Images
3. Push Images to Docker Hub
4. Deploy to Kubernetes
5. Update Kubernetes deployments

---

🛠️ Jenkins Pipeline Stages

```text
✔ Clone Repository
✔ Build Backend Docker Image
✔ Build Frontend Docker Image
✔ Docker Hub Login
✔ Push Backend Image
✔ Push Frontend Image
✔ Update Backend Image
✔ Update Frontend Image
✔ Deploy to Kubernetes
```

---

📜 Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        DOCKER_HUB = "nithinp004"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Nithin2906/devopsproject.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    bat '''
                    docker build -t %DOCKER_HUB%/pd-backend:%BUILD_NUMBER% -t %DOCKER_HUB%/pd-backend:latest .
                    '''
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    bat '''
                    docker build -t %DOCKER_HUB%/pd-frontend:%BUILD_NUMBER% -t %DOCKER_HUB%/pd-frontend:latest .
                    '''
                }
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    '''
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                bat '''
                docker push %DOCKER_HUB%/pd-backend:%BUILD_NUMBER%
                docker push %DOCKER_HUB%/pd-backend:latest
                '''
            }
        }

        stage('Push Frontend Image') {
            steps {
                bat '''
                docker push %DOCKER_HUB%/pd-frontend:%BUILD_NUMBER%
                docker push %DOCKER_HUB%/pd-frontend:latest
                '''
            }
        }

        stage('Update Backend Image') {
            steps {
                bat '''
                kubectl set image deployment/pd-backend pd-backend=nithinp004/pd-backend:%BUILD_NUMBER%
                '''
            }
        }

        stage('Update Frontend Image') {
            steps {
                bat '''
                kubectl set image deployment/pd-frontend pd-frontend=nithinp004/pd-frontend:%BUILD_NUMBER%
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                kubectl apply -f k8s/
                '''
            }
        }
    }
}
```

---

📦 Docker Images

Backend Image

```text
nithinp004/pd-backend
```

Frontend Image

```text
nithinp004/pd-frontend
```

---

📊 CI/CD Workflow

```text
GitHub
   ↓
Jenkins Pipeline
   ↓
Docker Build
   ↓
Docker Hub Push
   ↓
Kubernetes Deployment
   ↓
Application Live
```

---

🧪 Verification Commands

Check Docker Containers

```bash
docker ps
```

Check Kubernetes Pods

```bash
kubectl get pods
```

Check Kubernetes Services

```bash
kubectl get svc
```

Check Jenkins Pipeline

Open:

```text
http://localhost:9090
```

---

📸 Output Screens

* Jenkins Successful Pipeline
* Docker Running Containers
* Kubernetes Pods Running
* Frontend Running
* Backend Running

---

👨‍💻 Author

Nithin P

DevOps | Docker | Kubernetes | Jenkins | CI/CD

---

⭐ Conclusion

This project demonstrates a complete DevOps CI/CD workflow by integrating:

* Docker containerization
* Jenkins automation
* Docker Hub registry
* Kubernetes orchestration

to achieve automated deployment and scalable application management.
