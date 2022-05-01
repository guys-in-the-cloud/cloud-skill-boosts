# Build a Website on Google Cloud: Challenge Lab

[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)

## Defining some variables given by Cloud Skill Boosts

```
export MONOLITH_IDENTIFIER=
```
example variable defination - export MONOLITH_IDENTIFIER=<Monolith_Identifier_given_in_the_lab_instructions> 

```
export CLUSTER_NAME=
```
example variable defination - export CLUSTER_NAME=<Cluster_Name_given_in_the_lab_instructions>

```
export ORDERS_IDENTIFIER=
```
example variable defination - export ORDERS_IDENTIFIER=<Orders_Identifier_name_given_in_the_lab_instructions>

```
export PRODUCTS_IDENTIFIER=
```
example variable defination - export PRODUCTS_IDENTIFIER=<Products_Identifier_given_in_the_lab_instructions>

```
export FRONTEND_IDENTIFIER=
```
example variable defination - export FRONTEND_IDENTIFIER=<Frontend_Identifier_name_given_in_the_lab_instructions>

## prerequisites 

before starting this lab, there are some APIs, we need to enable to specific service like Cloud Build etc.

```
gcloud services enable cloudbuild.googleapis.com
gcloud services enable container.googleapis.com
```

## Task 1: Download the monolith code and build your container

1.1 Clone the application code & scripts
```
git clone https://github.com/googlecodelabs/monolith-to-microservices.git
```

1.2 change to script directory & setup environment for application by running script
```
cd ~/monolith-to-microservices
./setup.sh
```

1.3 change to application directory & Build & push your application container iamge to the Google Container Registry
```
cd ~/monolith-to-microservices/monolith
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/${MONOLITH_IDENTIFIER}:1.0.0 .
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
kubectl expose deployment $FRONTEND_IDENTIFIER--type=LoadBalancer --port 80 --target-port 8080
```
7.2 Run the below command and wait till you will get the external ip address (ctrl + c to exit)
```
kubectl get svc -w
```

# Congratulations! you've completed your challenge lab
## Happy Learning
## See you in the cloud...
