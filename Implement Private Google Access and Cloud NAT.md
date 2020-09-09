<h2>LAB:Implement private google access and cloud NAT</h2>

**Set Default Project**

`gcloud config set project qwiklabs-gcp-01-da239d2d81e6`

------

<h2>Task 1. Create the VM instance</h2>

**Create a VPC network and firewall rules**
`gcloud compute networks create privatenet --subnet-mode=custom`

`gcloud compute networks subnets create privatenet-us --network privatenet --range 10.130.0.0/20 --region us-central1`

`gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24`

`gcloud compute firewall-rules create privatenet-allow-ssh --network privatenet --action=ALLOW --direction=INGRESS --source-ranges=35.235.240.0/20 --rules=tcp:22 --priority=100`



**Create the VM instance with no public IP address**

`gcloud compute instances create vm-internal --zone us-central1-c --machine-type=n1-standard-1 --network privatenet --subnet privatenet-us --no-address`



**SSH to vm-internal to test the IAP tunnel**
`gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap`



**To test the external connectivity of vm-internal, run the following command**
`ping -c 2 www.google.com`

------


<h2>Task 2. Enable Private Google Access</h2>

**Create a Cloud Storage bucket**
`gsutil mb -c multi_regional gs://qwiklabs-gcp-01-da239d2d81e6`



**Copy an image file into your bucket**
`gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://qwiklabs-gcp-01-da239d2d81e6`

`gsutil cp gs://qwiklabs-gcp-01-da239d2d81e6/*.svg .`



**To connect to vm-internal, run the following command**
`gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap`



**Enable Private Google Access**
**Run the following command to enable Private Google Access:**
`gcloud compute networks subnets update privatenet-us --region=us-central1 --enable-private-ip-google-access`



**Verify that Private Google Access is enabled by running this command:**
`gcloud compute networks subnets describe privatenet-us --region=us-central1 --format="get(privateIpGoogleAccess)"`

------

<h2>Task 3. Configure a Cloud NAT gateway</h2>

**Create NAT router**
`gcloud compute routers create nat-router --network=privatenet --region us-central1`



**Configure a Cloud NAT gateway**
`gcloud compute routers nats create nat-config --router=nat-router --region us-central1 --auto-allocate-nat-external-ips --nat-custom-subnet-ip-ranges=privatenet-us`

------

<h2>Task 4. Configure and view logs with Cloud NAT Logging</h2>
`gcloud compute routers nats update nat-config --router=nat-router --region=us-central1 --enable-logging`

------

**To view NAT logs generated**
`gcloud logging read 'resource.type=nat_gateway' --limit=10 --format=json`

------

















