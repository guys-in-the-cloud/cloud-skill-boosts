# Scale Out and Update a Containerized Application on a Kubernetes Cluster

[YouTube Video Link](*)

## prerequisites 

1 Before starting this lab, we need to download the source code of the application which is given in the lab instruction.
```
gsutil cp gs://$DEVSHELL_PROJECT_ID/echo-web-v2.tar.gz .
```
2 Extract the downloaded application file 
```
tar -xvf echo-web-v2.tar.gz
```

## Task 1: Build a echo-app:v2 tagged Docker Image & Push the image to the Google Container Registry
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v2 .
```

## Task 2: Deploy the echo-app:v2 application image to the Kubernetes Cluster

3.1 Connect to the previously created cluster so deployment of application
```
gcloud container clusters get-credentials echo-cluster --zone us-central1-a --project $DEVSHELL_PROJECT_ID
```

3.2 Deploy the sample application image, which we created in the previous step
```
kubectl create deployment echo-web --image=gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v2
```
3.3 Expose your deployment by creating a service
```
kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
```
## Task 2: Scale your deployment to 2
```
kubectl scale deploy echo-web --replicas=2
```
## Task 4: Validate the application, It should working on v 2.0.0
```
kubectl port-forward service/echo-web 8080:80
```
after this command, click on web preview on port 8080 & you can see in your browser that it is showing v 2.0.0
![image](https://user-images.githubusercontent.com/104570014/167699321-835f2885-3c0a-49bb-a4cf-0e1d290e5ca3.png)

# Congratulations! you've completed your challenge lab
## Happy Learning
## See you in the cloud...
