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
```
gsutil cp gs://spls/gsp335/gsp335.zip .
unzip gsp335.zip
```

## Task 2: Set up a cluster
```
gcloud container clusters create $CLUSTER_NAME \
   --zone us-central1-c \
   --machine-type n1-standard-4 \
   --num-nodes 2 \
   --enable-network-policy
```
## Creating a cloudSQL instance
```
gcloud sql instances create $CLOUD_SQL_INSTANCE --region us-central1
```
## Task 3: Set up WordPress
```
gcloud iam service-accounts create $SERVICE_ACCOUNT
```
```
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
   --member="serviceAccount:$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com" \
   --role="roles/cloudsql.client"
```
```
gcloud iam service-accounts keys create key.json --iam-account=$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com
```
```
kubectl create secret generic cloudsql-instance-credentials --from-file key.json
```
```
kubectl create secret generic cloudsql-db-credentials \
   --from-literal username=wordpress \
   --from-literal password='password'
```
```
kubectl create -f volume.yaml
```
```
kubectl apply -f wordpress.yaml
```
```
sed -i ""
```
