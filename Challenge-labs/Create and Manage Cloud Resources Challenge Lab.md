# Create and Manage Cloud Resources - Challenge Lab

[YouTube Video Link](https://www.youtube.com/watch?v=QTbwYBiqCsE)

## Defining some variables given by Cloud Skill Boosts

```
export VM_NAME=
```
example variable defination - <mark> export VM_NAME=<vm_name_given_in_the_lab_instructions> </mark>

```
export PORT=
```
example variable defination - export PORT=<port_given_in_the_lab_instructions>

```
export FIREWALL_RULE_NAME=
```
example variable defination - export FIREWALL_RULE_NAME=<firewall_rule_name_given_in_the_lab_instructions>

## Task 1: Create a project jumphost instance

```
gcloud compute instances create $VM_NAME \
--zone=us-east1-b \
--machine-type=f1-micro \
--image=projects/debian-cloud/global/images/debian-10-buster-v20220406 
```

## Task 2: Create a Kubernetes service cluster

1. Create a cluster
```
gcloud container clusters create nucleus-backend \
          --num-nodes 1 \
          --network nucleus-vpc \
          --region us-east1
```
2. Connect to your cluster for further deployments
```
gcloud container clusters get-credentials nucleus-backend \
          --region us-east1
```
3. Create hello-server deployment
```
kubectl create deployment hello-server \
          --image=gcr.io/google-samples/hello-app:2.0
```
4. Expose hello-server deployment with a loadbalancer type of service
```
kubectl expose deployment hello-server \
          --type=LoadBalancer \
          --port $PORT
```

## Task 3: Set up an HTTP load balancer

1. Create a start up script file, which configures nginx server 
```
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF

```
2. Create a instance template
```
gcloud compute instance-templates create web-server-template \
       --metadata-from-file startup-script=startup.sh \
       --network nucleus-vpc \
       --machine-type g1-small \
       --region us-east1
```
3. Create a target pool
```
gcloud compute target-pools create nginx-pool
```
4. Create instance group using previously created template
```
gcloud compute instance-groups managed create web-server-group \
       --base-instance-name web-server \
       --size 2 \
       --template web-server-template \
       --region us-east1
```
5. Create a firewall rule to allow traffic on port 80
```
gcloud compute firewall-rules create $FIREWALL_RULE_NAME \
       --allow tcp:80 \
       --network nucleus-vpc
```
6. Create a Health Check 
```
gcloud compute http-health-checks create http-basic-check
```
7. Create a Backend Service
```
gcloud compute instance-groups managed \
       set-named-ports web-server-group \
       --named-ports http:80 \
       --region us-east1
```
```
gcloud compute backend-services create web-server-backend \
       --protocol HTTP \
       --http-health-checks http-basic-check \
       --global
```
8. Adding previously created backend to backend service
```
gcloud compute backend-services add-backend web-server-backend \
       --instance-group web-server-group \
       --instance-group-region us-east1 \
       --global
```
9. Create a URL Map to serve you backend
```
gcloud compute url-maps create web-server-map \
       --default-service web-server-backend
```
10. create target proxy to your URL Map
```
gcloud compute target-http-proxies create http-lb-proxy \
       --url-map web-server-map
```
11. Create a forwarding rule to redirect traffic to the target proxy
```
gcloud compute forwarding-rules create permit-tcp-rule-261 \
     --global \
     --target-http-proxy http-lb-proxy \
     --ports 80
```
# Congratulations you've completed your challenge lab
## Happy Learning
