# Build and Deploy a Docker Image to a Kubernetes Cluster

[YouTube Video Link](*)

## prerequisites 

1 Before starting this lab, we need to download the source code of the application which is given in the lab instruction.
```
wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Build%20and%20Deploy%20a%20Docker%20Image%20to%20a%20Kubernetes%20Cluster/resources-echo-web.tar.gz
```
2 Extract the downloaded application file 
```
tar -xvf resources-echo-web.tar.gz
```
## Task 1: Create a Kubernetes Cluster 
```
gcloud container clusters create echo-cluster --num-nodes 2 --zone us-central1-a --machine-type n1-standard-2
```

## Task 2: Build a tagged Docker Image & Push the image to the Google Container Registry
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 .
```

## Task 3: Deploy the application to the Kubernetes Cluster

3.1 Connect to the previously created cluster so deployment of application
```
gcloud container clusters get-credentials echo-cluster --zone us-central1-a --project $DEVSHELL_PROJECT_ID
```
It will take approx 4-5 mints to create cluster so please be patience

3.2 Deploy the sample application image, which we created in the previous step
```
kubectl create deployment echo-web --image=gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1
```
3.3 Expose your deployment by creating a service
```
kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
```

# Congratulations! you've completed your challenge lab
## Happy Learning
## See you in the cloud...
