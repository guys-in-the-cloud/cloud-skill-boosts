# Scale Out and Update a Containerized Application on a Kubernetes Cluster

[YouTube Video Link](*)

## prerequisites 

1 Before starting this lab, we need to download the source code of the application which is given in the lab instruction.
```
gsutil cp gs://$DEVSHELL_PROJECT_ID/echo-web-v2.tar.gz .
```
2 Extract the downloaded application file 
```
tar -xvf resources-echo-web-v2.tar.gz
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
