# Build a Website on Google Cloud: Challenge Lab

[YouTube Video Link](*)

## prerequisites 

1 Before starting this lab, we need to download the source code of the application which is given in the lab instruction.
```
wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Build%20and%20Deploy%20a%20Docker%20Image%20to%20a%20Kubernetes%20Cluster/resources-echo-web.tar.gz
```
2 Extract the downloaded application file 
```
tar -xvf resources-echo-web.tar.g
```
## Task 1: Create a Kubernetes Cluster 
```
gcloud container clusters create echo-cluster --num-nodes 2 --zone us-central1-a --machine-type n1-standard-2
```

## Task 2: Build a tagged Docker Image & Push the image to the Google Container Registry

1.1 Build a tagged Docker Image
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 .
```

1.2 Push the image to the Google Container Registry
```
gcloud container clusters get-credentials echo-cluster --zone us-central1-a --project $DEVSHELL_PROJECT_ID
```

1.3 change to application directory & Build & push your application container iamge to the Google Container Registry
```
kubectl create deployment echo-web --image=gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1
```
```
kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
```
## Task 2: Create a kubernetes cluster and deploy the application

2.1 Setup your default zone for kubernetes deployment 
```
gcloud config set compute/zone us-central1-a
```
2.2 Create a cluster with 3 nodes
```
gcloud container clusters create $CLUSTER_NAME --num-nodes 3
```
It will take approx 4-5 mints to create cluster so please be patience

2.3 authenticate your cluster in your cloud shell, so that you can work with your cluster using kubectl 
```
gcloud container clusters get-credentials $CLUSTER_NAME
```

2.4 Deploy your application container image in created kubernetes cluster & expose it on port 80 with loadbalancer type of service
```
kubectl create deployment $MONOLITH_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/${MONOLITH_IDENTIFIER}:1.0.0
kubectl expose deployment $MONOLITH_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8080
```
2.5 Run the below command and wait till you will get the external ip address (ctrl + c to exit)
```
kubectl get svc -w
```

## Task 3: Create a containerized version of your Microservices

3.1 Build & push some more container iamge to the Google Container Registry, here we're creating orders service container image
```
cd ~/monolith-to-microservices/microservices/src/orders
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/${ORDERS_IDENTIFIER}:1.0.0 .
```
3.2 Build & push some more container iamge to the Google Container Registry, here we're creating products service container image
```
cd ~/monolith-to-microservices/microservices/src/products
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/${PRODUCTS_IDENTIFIER}:1.0.0 .
```

## Task 4: Deploy the new microservices

4.1 Deploy your orders container image in kubernetes cluster & expose it on port 80 with loadbalancer type of service
```
kubectl create deployment $ORDERS_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/${ORDERS_IDENTIFIER}:1.0.0
kubectl expose deployment $ORDERS_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8081
```
4.2Run the below command and wait till you will get the external ip address (ctrl + c to exit)
```
kubectl get svc -w
```
4.3 Deploy your orders container image in kubernetes cluster & expose it on port 80 with loadbalancer type of service
```
kubectl create deployment $PRODUCTS_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/${PRODUCTS_IDENTIFIER}:1.0.0
kubectl expose deployment $PRODUCTS_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8082
```
4.4 Run the below command and wait till you will get the external ip address (ctrl + c to exit)
```
kubectl get svc -w
```

## Task 5: Configure the Frontend microservice

5.1 exporting service ip addresses into variables  
```
export ORDERS_SERVICE_IP=$(kubectl get services -o jsonpath="{.items[1].status.loadBalancer.ingress[0].ip}")
export PRODUCTS_SERVICE_IP=$(kubectl get services -o jsonpath="{.items[2].status.loadBalancer.ingress[0].ip}")
```

5.2  replacing url with the service variables and build the application 
```
cd ~/monolith-to-microservices/react-app
sed -i "s/localhost:8081/$ORDERS_SERVICE_IP/g" .env
sed -i "s/localhost:8082/$PRODUCTS_SERVICE_IP/g" .env
npm run build
```

## Task 6: Create a containerized version of the Frontend microservice

6.1 Build & push some more container iamge to the Google Container Registry, here we're creating frontend service container image
```
cd ~/monolith-to-microservices/microservices/src/frontend
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/${FRONTEND_IDENTIFIER}:1.0.0 .
```

## Task 7: Deploy the Frontend microservice

7.1 Deploy your frontend container image in kubernetes cluster & expose it on port 80 with loadbalancer type of service
```
kubectl create deployment $FRONTEND_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/${FRONTEND_IDENTIFIER}:1.0.0
kubectl expose deployment $FRONTEND_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8080
```
7.2 Run the below command and wait till you will get the external ip address (ctrl + c to exit)
```
kubectl get svc -w
```

# Congratulations! you've completed your challenge lab
## Happy Learning
## See you in the cloud...
