# Task Manager API

This repository demonstrates a **production-like deployment setup** for the Task Manager API using Kubernetes and Docker.  
It focuses on the **DevOps workflow**: containerization, Deployment, Service, and Ingress.

> Application code lives in a separate repository: [TaskManager](https://github.com/panutka1/TaskManager)

---

## Architecture

Client → Ingress → Service → Deployment → Pod (FastAPI app) → In-memory storage (tasks list)

- The application runs inside a Docker container.  
- Kubernetes handles deployment with 2 replicas for high availability.  
- Ingress exposes the service through a local domain (`taskmanager.local`).


---

## Tech Stack

* Python 3.12  
* FastAPI  
* Uvicorn  
* Docker  
* Kubernetes (Minikube)  
* Nginx Ingress Controller  

---

## Project Structure

```
TaskManager_devops
│
├── deployment.yaml
├── ingress.yaml
├── service.yaml
└── README.md
```

---

## Running the Application

### 1. Clone repository

```
# Clone DevOps repo
git clone <repo-url> TaskManager_devops
cd TaskManager_devops

# Clone application repo
git clone https://github.com/panutka1/TaskManager
cd TaskManager
```

### 2. Build the Docker image from the TaskManager repository

```
docker build -t taskmanager:latest .
```

### 3. Start minikube

```
minikube start
```

### 4. Apply Kubernetes manifests

```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

### 5. Start Minikube tunnel (for Ingress)

```
minikube tunnel
```

---

## Accessing the API

### Root endpoint

```
http://taskmanager.local/
```

### Swagger UI

```
http://taskmanager.local/docs
```

### Health check

```
http://taskmanager.local/health-check
```

---

**Make sure to add the following line to /etc/hosts**

127.0.0.1 taskmanager.local

---

## Kubernetes Resources

Deployment:
* 2 replicas for high availability
* Image: taskmanager:latest
* Container port: 8000

Service:
* Type: ClusterIP
* Exposes Deployment internally on port 80

Ingress:
* Maps local domain taskmanager.local → service
* Path / routes to app-service

---

## Development Commands

### Apply manifests

```
kubectl apply -f <file>
```

### Delete resource

```
kubectl delete -f <file>
```

### Check pods

```
kubectl get pods
kubectl logs <pod-name>
```

### Scale deployment

```
kubectl scale deployment app-deployment --replicas=3
```

### Update deployment image

```
kubectl set image deployment/app-deployment app=taskmanager:v2
```



## Future Improvements

* ConfigMaps for environment configuration
* Secrets for sensitive data
* Horizontal Pod Autoscaler for automatic scaling
* PostgreSQL database for persistence
* HTTPS with Let's Encrypt
* Monitoring with Prometheus and Grafana

