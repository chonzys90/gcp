# gcp
# how to work with , in and around google cloud platform using command
# Overview
# Google Cloud Fundamentals: Getting Started with Compute Engine.Compute Engine provides tools to help you bring your existing applications to the cloud
# the following repository demonstrates how to set up VM in GCP  
# LAB ONE
# Objectives
# Task 1: Sign in to the Google Cloud Platform (GCP) Console
# Task 2: Create a virtual machine using the GCP Console
# 1stdisplay a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command gcloud compute zones list | grep followed by the region that Qwiklabs or your instructor assigned you to.]]]]]]]]]]]]]]
# check available zones in list format
gcloud compute zones list 
# set default zone to chosen one  us cental b
gcloud config set compute/zone us-central1-b
# creating a VM instance called my-vm-1 in above chosen zone
 gcloud compute instances create "my-vm-1" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
# Task 3: Create a virtual machine using the gcloud command line
# check available zones in list format
gcloud compute zones list | grep us-central1
# set default zone to chosen one  us cental b
gcloud config set compute/zone us-central1-b
# creating a VM instance called my-vm-2 in above chosen zone
/gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
# Task 4: Connect between VM instances
# on ssh fo my vm 2 instance
# use ping command to cornfirm that myVm2 can be reach myVm1 over network
ping my-vm-1
# note Ctrl+C to abort the ping command
# Use the ssh command to open a command prompt on my-vm-1:
ssh my-vm-1
# note yes to continue
sudo apt-get install nginx-light -y
# At the command prompt on my-vm-1, install the Nginx web server
sudo nano /var/www/html/index.nginx-debian.html
# Use the nano text editor to add a custom message to the home page of the web server
# nb you can rename if you want
# confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command
curl http://localhost/
exit
# Username
# student-03-2042c26db66f@qwiklabs.net
# Password-5cTDLx69t2L2
# GCP Project ID-qwiklabs-gcp-03-1e74f4b15791
# Region us-central1
# Zone us-central1-a 


# LAB TWO
# working with Kubernetes on Gcp
# Cloud Fundamentals: Getting Started with GKE
# overview
# Provision a Kubernetes cluster using Kubernetes Engine.
# Deploy and manage Docker containers using kubectl.before you start confirm that both of these APIs are enabled:
# Kubernetes Engine API
# Container Registry API
# Objectives
# Task 1: Sign in to the Google Cloud Platform (GCP) Console
# Task 2: Confirm that needed APIs are enabled
# Task 3: Start a Kubernetes Engine cluster
# NOTE -Open Cloud Shell button.
# For convenience,zone  assigned  to  environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:
export MY_ZONE=us-central1-a
# Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
# nb=It takes several minutes to create a cluster as Kubernetes Engine 
# After the cluster is created, check your installed version of Kubernetes using the kubectl version command:
kubectl version
# nb=The gcloud container clusters create command automatically authenticated kubectl for you
# Task 4: Run and deploy a container
# From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)
kubectl create deploy nginx --image=nginx:1.17.10
# nb==In Kubernetes, all containers run in pods. This use of the kubectl create command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.
# Note: If you see any deprecation warning about future version you can simply ignore it for now and can proceed furthe==[[[[
# View the pod running the nginx container:
kubectl get pods
# Expose the nginx container to the Internet:
kubectl expose deployment nginx --port 80 --type LoadBalancer
# Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod]]]]
# View the new service:
kubectl get services
# Scale up the number of pods running on your service:
kubectl scale deployment nginx --replicas 3
# Confirm that Kubernetes has updated the number of pods:
kubectl get pods
# Confirm that your external IP address has not changed
kubectl get services
# Username student-02-ec8ea7fb765e@qwiklabs.net
# Password Z8VwJ9p9gk
# GCP Project ID qwiklabs-gcp-02-d1beb820cd65
# Region us-central1
# Zone us-central1-a

# The following is an example of a Deployment manifest file in YAML format:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
        
# To get detailed information about the Deployment, run the following command
kubectl describe deployment [DEPLOYMENT_NAME]
# To list the Pods created by the Deployment, run the following command:
kubectl get pods -l [KEY]=[VALUE]
# To view a Deployment's manifest, run the following command:
kubectl get deployments [DEPLOYMENT_NAME] -o yaml
# afeter reaching this point you should have bsome knowledg of how to work with GCP AND DEPLOY



