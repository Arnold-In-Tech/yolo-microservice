# Orchestration with Google Kubernetes Engine : IP4

--------------------------------------------------------------------------------------------------------

Access the deployed application via the live `EXTERNAL-IP` : <a href="http://34.122.212.56:3000" target="_blank">**http://34.122.212.56:3000**</a>

The deployed application looks like:


![Yolo front-end](./front-end-image.png?raw=true)


---

## Technology Stack

![Google Cloud](https://img.shields.io/badge/GoogleCloud-%234285F4.svg?style=for-the-badge&logo=google-cloud&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)

---

## A. Test-Run on Minikube

### (i) Start Minikube Cluster
```sh
minikube start
```

Check the status of the Minikube cluster to confirm it is running:
```sh
minikube status
```

### (ii) Deploy the Application

- **Backend**:
```sh
kubectl apply -f minikube-test-deployment/backend-deployment.yaml
```
- **Frontend**:
```sh
kubectl apply -f minikube-test-deployment/frontend-deployment.yaml
```
- **Database**:
```sh
kubectl apply -f minikube-test-deployment/mongoDB-deployment.yaml
```

Confirm the deployments are running:
```sh
kubectl get pods
```

### (iii) Access the Frontend

Minikube doesn't provide an external IP. To access the frontend, run:
```sh
kubectl get svc
minikube service ahnoamu-yolo-client
```
This will open the frontend in your default browser.

### (iv) Delete the Resources

Once done, clean up with:
```sh
minikube delete
```

#### Service Communication

- The backend connects to MongoDB via the service name `app-ip-mongo`.
- The frontend connects to the backend via the service name `ahnoamu-yolo-backend`.
- Kubernetes manages container networking automatically, so no need to define explicit networks.

---

## B. Orchestration on Google Kubernetes Engine (GKE)

### Prerequisites

1. **Google Cloud Account**: You need a Google Cloud account and project.
2. **GKE Cluster**: Ensure a GKE cluster is created in your project.
3. **kubectl**: Install and configure `kubectl` to access your GKE cluster.
4. **Persistent Disk**: GKE requires `PersistentVolume` backed by a GCP `PersistentDisk` (not `hostPath`).

### Steps to Deploy

#### 1. Set Up GKE Cloud Environment

Activate Cloud Shell and authenticate:
```bash
gcloud auth list
gcloud auth login
```

Set your Google Cloud project and compute region/zone:
```bash
gcloud config set account `ACCOUNT`
gcloud config list project
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
```

#### 2. Create GKE Cluster

Create a GKE cluster with a 50GB disk size:
```bash
gcloud container clusters create my-cluster --machine-type=e2-medium --zone us-central1-a --disk-size 50GB
```

Get credentials for the new cluster:
```bash
gcloud container clusters get-credentials my-cluster --zone us-central1-a
```

#### 3. Create Persistent Disk for GKE

Create a 10GiB Persistent Disk:
```bash
gcloud compute disks create my-mongo-disk --size=10GiB --zone=us-central1-a --type=pd-standard
```

Verify disk creation:
```bash
gcloud compute disks list
```

Modify `mongoDB-deployment.yaml` to use the GCP PersistentDisk instead of `hostPath`.

#### 4. Deploy to GKE

Apply the Kubernetes configuration files:
```bash
kubectl apply -f mongoDB-deployment.yaml
kubectl apply -f backend-deployment.yaml
kubectl apply -f frontend-deployment.yaml
```

Check the status of the deployments:
```bash
kubectl get pods
```

#### 5. Access the Services

Once deployed, get the external IPs:
```bash
kubectl get svc
```

Access the frontend via the live `EXTERNAL-IP`: <a href="http://34.122.212.56:3000" target="_blank">**http://34.122.212.56:3000**</a>

#### Scaling

To scale the deployments, adjust the `replicas` field in the deployment YAML files.

---

## Author

Arnold .A.

## License

All assets and code are licensed under the [MIT License](https://choosealicense.com/licenses/mit/) unless otherwise specified.

---


