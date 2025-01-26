
# Orchestration with Google Kubernetes Engine : IP4

---
## A. Test-run the ochestration locally on a minikube cluster

**(i). Create a minikube cluster:**

```sh
minikube start
```

Confirm minikube cluster is running:
```sh
minikube status
```

**(ii). Deploy app**

- Deploy the backend

```sh
kubectl apply -f minikube-test-deployment/backend-deployment.yaml
```

- Deploy the frontend
	
```sh
kubectl apply -f minikube-test-deployment/frontend-deployment.yaml
```

- Deploy the database

```sh
kubectl apply -f minikube-test-deployment/mongoDB-deployment.yaml
```


Confirm the deployments are running:	
```sh
kubectl get pods
```


**(iii). To access the frontend outside the cluster run the command below:**

(N/B: minikube does not provide an external IP). 

```sh
kubectl get svc
```

```sh
minikube service ahnoamu-yolo-client 
```
The above command will automatically open the frontend in your default web browser using the Minikube IP and assigned NodePort.


**(iv) Delete the resource thereafter.**

```sh
minikube delete 
```
 

#### Service Communication 
- The backend connects to MongoDB using the Mongo service name (app-ip-mongo), and the frontend connects to the backend using the backend service name (ahnoamu-yolo-backend).

- Cluster Networking: Kubernetes manages the network for containers automatically, so no need to explicitly define networks like in Docker Compose.


--------------------------------------------------------------------------------------------------------



-------------------------------------------------------------------------------------------------------- 

## B. Orchestration on a GKE (Google Kubernetes Engine)

To deploy your backend, database, and frontend services within a cloud native environment like Google Kubernetes Engine (GKE):

### Prerequisites:

1. **Google Cloud Platform Account**: You need a Google Cloud account and project.
2. **GKE Cluster**: Ensure you have a GKE cluster created in your project. If not, you can create one using the GCP Console or `gcloud` CLI.
3. **kubectl**: Ensure you have `kubectl` installed and configured to access your GKE cluster.
4. **Persistent Disk**: GKE doesn’t support `hostPath` volumes in production. Use `PersistentVolume` backed by a GCP `PersistentDisk` instead.

---

### Steps to Deploy:

#### 1. Set up GKE cloud environment:

Open cloud console

Activate cloud shell

Authorise active account
```bash
gcloud auth list
```
Cloud Shell needs permission to use your credentials for the gcloud CLI command. Authorize, else login using:
   
```bash
gcloud auth login
```
This will open a web browser where you can log in to your Google Cloud account.

```bash
gcloud config set account `ACCOUNT`
```
	
List the project ID with this command:
```bash	
gcloud config list project	
```

Set a default compute zone

- Set the default compute region:

```bash
gcloud config set compute/region us-central1	
```
- Set the default compute zone:
```bash
gcloud config set compute/zone us-central1-a
```


#### 2. Create and set up GKE cluster:

Create a GKE cluster of 50GB disc size:
   
```bash
gcloud container clusters create my-cluster   --machine-type=e2-medium   --zone us-central1-a   --disk-type pd-standard   --disk-size 50GB
```

After the cluster is created, get authentication credentials for the cluster to interact with it.

```bash
gcloud container clusters get-credentials my-cluster --zone us-central1-a
```


#### 3. Create and set up Persistent Volume (PV) for GKE:
   
GKE doesn’t support `hostPath` as a storage option for production workloads. A Persistent Disk is used instead. 
Create a Persistent Disk in the cloud, then modify the `PersistentVolume` definition on mongoDB-deployment.yaml to use a GCP `PersistentDisk`:


Create a Persistent Disk using the gcloud CLI with the following commands:

**(i). Create the Persistent Disk**:

Use the following command to create a 10GiB persistent disk named `my-mongo-disk` in your desired region and zone (i.e., `us-central1-a`):

```bash
gcloud compute disks create my-mongo-disk \
  --size=10GiB \
  --zone=us-central1-a \
  --type=pd-standard
```

- The `--type=pd-standard` option creates a standard persistent disk. If you want to use an SSD-backed disk, replace `pd-standard` with `pd-ssd`.


**(iii). Verify the Disk Creation**:

To confirm the disk was created, you can list your disks with:

```bash
gcloud compute disks list
```
- This will display all the disks in your project and their status.

**(iv). Update the yaml file for GKE:**

- Instead of using `hostPath`, modify the `PersistentVolume` to use a GCP `PersistentDisk` on mongoDB-deployment.yaml

- Both the backend and frontend services are set to `LoadBalancer`. For the frontend, the service is exposed on port `3000`, and for the backend, it's on port `5000`.

- The MongoDB service uses `ClusterIP` so that it's only accessible within the cluster, as intended.



#### 4. Deploy to GKE:

**(i). Apply the Kubernetes Config**:
Apply the configuration to your GKE cluster using `kubectl`:

```bash
kubectl apply -f mongoDB-deployment.yaml
kubectl apply -f backend-deployment.yaml
kubectl apply -f frontend-deployment.yaml
```

**(ii). Check the Status**:
Monitor the status of your deployments:

```bash
kubectl get pods
```

**(iii). Accessing Services**:
Once deployed, you can access the frontend and backend using the external IP addresses provisioned by the `LoadBalancer` services. Use the following command to get the external IPs:

```bash
kubectl get svc
```
   
The front-end service can be accessed via the following live `EXTERNAL-IP` : [**http://34.122.212.56:3000**](http://34.122.212.56:3000)


- **Scaling**: You can scale the deployments by changing the `replicas` field in the `Deployment` YAML.

-------------------------------------------------------------------------------------------------------- 

The deployed image looks like below:
![Yolo front-end](./front-end-image.png?raw=true)

-------------------------------------------------------------------------------------------------------- 

# Author
Arnold .A.


# Licence
All assets and code are under the [MIT](https://choosealicense.com/licenses/mit/) LICENSE and in the public domain unless specified otherwise.



