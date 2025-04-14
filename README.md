# 🚀 Deploying a React Web Application on Kubernetes using Minikube

This repository demonstrates how to create, Dockerize, and deploy a React web application using Kubernetes on a local environment powered by Minikube. It’s designed for **absolute beginners** and includes installation guides, deployment commands, and troubleshooting tips.

---

## 🧱 Kubernetes Architecture

> Replace with your actual Kubernetes architecture diagram.
> 
> Example: Add a diagram showing user → NodePort → Service → Deployment → Pods.

![Kubernetes Architecture](images/kubernetes-architecture.png)

---

## 📚 Table of Contents

- [📋 Prerequisites](#-prerequisites)
- [⚙️ Installation Guide](#️-installation-guide)
  - [1️⃣ Install Docker](#1️⃣-install-docker)
  - [2️⃣ Install Minikube](#2️⃣-install-minikube)
  - [3️⃣ Install kubectl](#3️⃣-install-kubectl)
- [🧪 Local Deployment](#-local-deployment)
  - [1️⃣ Create React App](#1️⃣-create-react-app)
  - [2️⃣ Dockerize Application](#2️⃣-dockerize-application)
  - [3️⃣ Kubernetes Setup](#3️⃣-kubernetes-setup)
  - [4️⃣ Deploy to Minikube](#4️⃣-deploy-to-minikube)
- [🌍 Global Deployment Options](#-global-deployment-options)
- [🐛 Troubleshooting](#-troubleshooting)
- [📚 References](#-references)
- [✅ Final Result](#-final-result)

---

## 📋 Prerequisites

Before proceeding, ensure your system meets the following requirements:

- ✅ 2 GB RAM or more
- ✅ 20 GB of free disk space
- ✅ Administrator/root access
- ✅ Stable internet connection

---

## ⚙️ Installation Guide

### 1️⃣ Install Docker

#### Windows/macOS:
- Download Docker Desktop from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
- Follow the installation wizard
- Enable **Kubernetes** in Docker Desktop settings

> 🖼️ *Replace below with your Docker installation screenshot*
> 
> ![Docker Install](images/docker-install.png)


# Verify Docker:
docker --version
2️⃣ Install Minikube
Official Installation: https://minikube.sigs.k8s.io/docs/start/

# For Linux:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
Start Minikube:

 
minikube start
3️⃣ Install kubectl
Official Guide: https://kubernetes.io/docs/tasks/tools/

For Linux:

 
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
🧪 Local Deployment
 # Create React App
 
npx create-react-app my-react-app  
cd my-react-app  
npm start
🖼️ Add screenshot of the running React app on http://localhost:3000

2️⃣ Dockerize Application
Create a production build:
 
npm run build
Create a Dockerfile:

Dockerfile
 
FROM nginx:alpine
COPY build/ /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
Use Minikube's Docker daemon:

 
eval $(minikube -p minikube docker-env)
Build Docker image:

 
docker build -t my-react-app .
Verify:

 
docker images
3️⃣ Kubernetes Setup
Create deployment.yaml:

yaml
Copy
Edit
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-react-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-react-app
  template:
    metadata:
      labels:
        app: my-react-app
    spec:
      containers:
        - name: my-react-app
          image: my-react-app
          ports:
            - containerPort: 80
          imagePullPolicy: IfNotPresent
# Create service.yaml:

yaml
Copy
Edit
apiVersion: v1
kind: Service
metadata:
  name: my-react-app-service
spec:
  type: NodePort
  selector:
    app: my-react-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30036
# 4️⃣ Deploy to Minikube
 
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
Access the app:

 
minikube ip
Open in browser:

 
http://<minikube-ip>:30036
🖼️ Add screenshot of your running app via Minikube URL

🌍 Global Deployment Options
Option 1: Cloud Kubernetes (GKE, EKS, AKS)
Push image to DockerHub:

 
docker tag my-react-app your-dockerhub-username/my-react-app
docker push your-dockerhub-username/my-react-app
Update deployment.yaml with full image path and deploy on:

Google GKE

AWS EKS

Azure AKS

Option 2: Static Hosting (For Static Sites)
 
npm run build
Drag the build/ folder to:

 ## Vercel

## Netlify

🐛 Troubleshooting
Problem: ImagePullBackOff

✔️ Rebuild inside Minikube Docker:

 
eval $(minikube -p minikube docker-env)
docker build -t my-react-app .
✔️ Check Image Name: Ensure it matches in deployment.yaml.

✔️ Delete old pods:

 
kubectl delete pod -l app=my-react-app
✔️ Describe Pod:

 
kubectl describe pod <pod-name>
📚 References
Minikube Docs

Docker Docs

Kubernetes Docs

React Docs

# ✅ Final Result
🖼️ Add a screenshot of your deployed application in the browser with Minikube IP and port

Your React app should now be running locally on Kubernetes using Minikube and ready to deploy globally!




