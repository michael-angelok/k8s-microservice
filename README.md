
# Kubernetes Microservice (Python + Docker + k3d + Ingress)

This project demonstrates how to containerize a Python microservice and deploy it to a local Kubernetes cluster using k3d.  
It covers the core DevOps concepts required for modern cloud environments:

- Docker image creation
- Kubernetes Deployment and Service
- NodePort vs. Ingress routing
- Local development cluster using k3d
- Traffic flow from Ingress -> Service -> Pod -> Container

The goal is to provide a clean, minimal, production-like example of deploying a microservice to Kubernetes.

## Project Structure

```

src/app.py          # Python microservice
Dockerfile          # Container definition
k8s/deployment.yml  # Kubernetes Deployment
k8s/service.yml     # NodePort Service
k8s/ingress.yml     # Ingress configuration (Traefik)

```

## Running the Project

### 1. Create k3d cluster with Ingress ports exposed
```bash
k3d cluster create dev \
  --port "80:80@server:0" \
  --port "443:443@server:0"
````

### 2. Build the Docker image

```bash
docker build -t micro-app .
```

### 3. Import the image into the cluster

```bash
k3d image import micro-app -c dev
```

### 4. Apply Kubernetes manifests

```bash
kubectl apply -f k8s/
```

### 5. Add local DNS entry

```
127.0.0.1   myapp.local
```

Access the application at:

```
http://myapp.local
```

## Traffic Flow

```
Browser (myapp.local)
      V
Ingress (Traefik)
      V
Service (app-service:80)
      V
Pod(s)
      V
Container (port 8000)
```

## What This Project Demonstrates

* How Ingress routing works in a local Kubernetes cluster
* How Services route traffic to Pods via labels
* Why k3d requires manual port exposure (80/443)
* How to import Docker images into a k3d cluster
* How to deploy and expose a microservice end-to-end

## Author

Michael-Angelo Karpowicz
