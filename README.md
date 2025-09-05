# 📖 Blog Application (Kubernetes Microservices)

This project is a **microservices-based blog application** deployed on a **local Kubernetes cluster (k3d)**.  
It demonstrates containerization, service orchestration, and scaling with Kubernetes.  

The application consists of three main components:  
- **Auth Service** – manages authentication and users.  
- **Blog Service** – manages blog posts.  
- **Blog Client (Frontend)** – React-based UI for users.  

---

## 🏗️ Architecture

- **Blog Client (Frontend)**  
  - Built with React.  
  - Deployed as `blog-client` pods.  
  - Exposed via **LoadBalancer service** on port `80`.  
  - Connects to backend APIs.  

- **Auth Service**  
  - Nest.js backend handling authentication & user management.  
  - Runs on port `3000`.  
  - Exposed as a **ClusterIP service** (`auth-service`).  

- **Blog Service**  
  - Node.js backend handling blog post CRUD operations.  
  - Runs on port `4000`.  
  - Exposed as a **ClusterIP service** (`blog-service`).  

- **Local Registry & Cluster**  
  - Images stored in **local Docker registry** (`localhost:5000`).  
  - Deployed to **k3d Kubernetes cluster** with multiple nodes.  

---

## 📂 Project Structure

```bash
.
├── k8s
    └── all-services.yaml  # k8s config
├── auth-service/          # Auth service (Nest.js backend)
├── blog-service/          # Blog service (Node.js backend)
├── blog-client/           # React frontend
├── README.md              # Project documentation
├── docker-compose.yml     # Docker compose

```


---
## ⚙️ Prerequisites

- [Docker](https://docs.docker.com/get-docker/)  
- [k3d](https://k3d.io/)  
- [kubectl](https://kubernetes.io/docs/tasks/tools/)  
- Node.js & npm (for local dev only)

---

## 🚀 Setup & Deployment


### 1. Create k3d Cluster
```bash
k3d cluster create blog-demo --agents 2 --servers 1 \
  --registry-create k3d-registry.localhost:0.0.0.0:5000
```

### 2. Build and Push Docker Images
```bash
# Auth Service
docker build -t localhost:5000/auth-service:1.0 ./auth-service
docker push localhost:5000/auth-service:1.0

# Blog Service
docker build -t localhost:5000/blog-service:1.0 ./blog-service
docker push localhost:5000/blog-service:1.0

# Blog Client
docker build -t localhost:5000/blog-client:1.0 ./blog-client
docker push localhost:5000/blog-client:1.0

### 3. Apply Kubernetes Manifests
kubectl apply -f all-services.yaml
```
### 3. Port Mapping
```bash
# Forward Blog Client (React frontend)
kubectl port-forward svc/blog-client 3001:80


# Forward Auth Service
kubectl port-forward svc/auth-service 3000:3000


# Forward Blog Service
kubectl port-forward svc/blog-service 4000:4000
```

## 🚀 Run the Application
### Application can be accessed via http://localhost:3001
