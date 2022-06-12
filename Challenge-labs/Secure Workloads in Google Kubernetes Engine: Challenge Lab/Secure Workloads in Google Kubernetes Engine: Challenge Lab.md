## Secure Workloads in Google Kubernetes Engine: Challenge Lab

[YouTube Video Link](https://youtu.be/vJKekSjT_Fk)

## Defining some variables given by Cloud Skill Boosts

```
export CLUSTER_NAME=
```

```
export CLOUD_SQL_INSTANCE=
```
```
export SERVICE_ACCOUNT=
```
## Task 1: Download the necessary files
Task 1: Setup Cluster
gsutil cp gs://spls/gsp335/gsp335.zip .
unzip gsp335.zip
search kubernetes engine and open in seprate tab similarly search sql


gcloud container clusters create $CLUSTER_NAME \
   --zone us-central1-c \
   --machine-type n1-standard-4 \
   --num-nodes 2 \
   --enable-network-policy


gcloud sql instances create $CLOUD_SQL_INSTANCE Instance  --region us-central1


refresh both new windows and check the work and wait until you get green check mark
************************************************************************
Task 2: Setup wordpress
Create database - wordpress
Add user - wordpress (no password)


Service account
gcloud iam service-accounts create $SERVICE_ACCOUNT


gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
   --member="serviceAccount:$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com" \
   --role="roles/cloudsql.client"


gcloud iam service-accounts keys create key.json --iam-account=$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com


kubectl create secret generic cloudsql-instance-credentials --from-file key.json


kubectl create secret generic cloudsql-db-credentials \
   --from-literal username=wordpress \
   --from-literal password=''


Create the WordPress deployment and service


kubectl create -f volume.yaml


- go to editor and reolace isntance name with sql instance name 
- save 


kubectl apply -f wordpress.yaml


************************************************************************
Task 3: Setup Ingress with TLS
helm version


helm repo add stable https://charts.helm.sh/stable
helm repo update


helm install nginx-ingress stable/nginx-ingress --set rbac.create=true


kubectl get service


. add_ip.sh  


student0047f4ad80a2dd.labdns.xyz (save it for latter use for host name)


kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v0.16.0/cert-manager.yaml


kubectl create clusterrolebinding cluster-admin-binding \
   --clusterrole=cluster-admin \
   --user=$(gcloud config get-value core/account)


goto editor and edit issuer.yaml to include lab email address


kubectl apply -f issuer.yaml


goto editor and edit ingress.yaml to include dns address received as output from . add_ip.sh


kubectl apply -f ingress.yaml


************************************************************************
Task 4: Set up Network Policy
goto editor and in network-policy.yaml add to end
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-world-to-nginx-ingress
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: nginx-ingress
  policyTypes:
  - Ingress
  ingress:
  - {}


kubectl apply -f network-policy.yaml
************************************************************************
Task 5: Setup Binary Authorization
goto security - Binary authorisatioin enableit and click on edit policy under specific rule select gke rule
- configure policy 
- disallow all images
- create specific rules, select cluster
- add specific rule, type us and select from dropdown, click add
- custom expetion path 
- add iamge paths given 
- save policy 
enanble binary authorisation for kuberetes clusater
************************************************************************
TAsk 6`
edit psp-restrictive.yaml 
line 2 change extensions/v1beta1 to policy/v1beta1
kubectl apply -f psp-restrictive.yaml
kubectl apply -f psp-role.yaml
kubectl apply -f psp-use.yaml
