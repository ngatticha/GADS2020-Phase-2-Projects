<h2>AK8S-03 Creating a GKE Cluster via GCP Console</h2>



**Set Default project**

`gcloud config set project qwiklabs-gcp-00-63ac733e0e84`

------

**Task 1. Deploy GKE clusters**
`gcloud container clusters create standard-cluster-1 --zone us-central1-a`

**View the cluster details**

`gcloud container clusters describe standard-cluster-1 --zone us-central1-a`

------

**Task 2. Modify GKE clusters**
`gcloud container clusters resize standard-cluster-1 --zone us-central1-a --node-pool default-pool --num-nodes 4`

------

**Task 3. Deploy a sample workload**
`kubectl create deployment nginx-1 --image=nginx`

------

**Task 4. View details about workloads in the GCP Console**

`kubectl get pods nginx-1-75969c956f-8lgcm -o yaml`
`kubectl describe pods nginx-1-75969c956f-8lgcm`

------