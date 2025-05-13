
#  Docker Build and Deploy Steps

## 1. Build and Push Docker Image

```
docker build -t pasindufdo98/docker_web_app_9p:v6 .
docker push pasindufdo98/docker_web_app_9p:v6
```

---

## 2. Apply Kubernetes Configurations

```
kubectl apply -f mongo-persistentVolume.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo-headlessService.yaml
kubectl apply -f mongo-statefulset.yaml
kubectl apply -f docker-web-app-service.yaml
kubectl apply -f docker-web-app-deployment.yaml
```

---

##  Kubernetes Files Brief Explanation

### 1. `mongo-persistentVolume.yaml`
Defines the Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) for MongoDB.  
Ensures each MongoDB pod has its own storage, and the data persists even if a pod restarts.

### 2. `mongo-secret.yaml`
Creates a Kubernetes Secret to securely store the MongoDB username and password.  
Credentials are passed securely as environment variables.

### 3. `mongo-headlessService.yaml`
Creates a Headless Service for MongoDB pods.  
Helps MongoDB nodes discover each other for ReplicaSet formation.

### 4. `mongo-statefulset.yaml`
Defines the StatefulSet for MongoDB, creating three pods (`mongo-0`, `mongo-1`, `mongo-2`).  
Ensures each MongoDB instance has stable identity and storage for replication.

### 5. `docker-web-app-service.yaml`
Creates a NodePort Service to expose the Node.js application externally.  
Maps internal port 3000 to external port 30080 for browser or API access.

### 6. `docker-web-app-deployment.yaml`
Deploys the Node.js web application into Kubernetes.  
Pulls the Docker image from DockerHub, connects to MongoDB, and runs the app in a pod.

---

MongoDB runs as a **ReplicaSet** (for redundancy and reliability).  
Node.js app runs on **port 3000**, exposed via **NodePort 30080**.

---

# API Endpoints

| Method | URL | Purpose |
|:-------|:----|:--------|
| `GET` | `/` | Home page (hello world) |
| `POST` | `/create` | Insert a new user |
| `GET` | `/read` | Retrieve users |
| `PUT` | `/update` | Update a user's email |
| `DELETE` | `/delete` | Delete a user |

---

# Example API Testing

## Create User

```
Invoke-RestMethod -Method POST -Uri http://localhost:30080/create `
-Headers @{ "Content-Type" = "application/json" } `
-Body '{"name":"Pasindu","email":"pasindu@example.com"}'
```

---

## Read Users

Open in browser:

```
http://localhost:30080/read
```

---

## Update User(in powershell)

```
Invoke-RestMethod -Method PUT -Uri http://localhost:30080/update `
-Headers @{ "Content-Type" = "application/json" } `
-Body '{"name":"Pasindu","newEmail":"pasindu_updated@example.com"}'
```

---

## Delete User(in powershell)

```
Invoke-RestMethod -Method DELETE -Uri http://localhost:30080/delete `
-Headers @{ "Content-Type" = "application/json" } `
-Body '{"name":"Pasindu"}'
```
