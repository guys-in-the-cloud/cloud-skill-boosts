# Ensure Access & Identity in Google Cloud: Challenge Lab

[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)

## Let's start with defining some variables given by Cloud Skill Boosts

```
export CUSTOM_SECURIY_ROLE=
```
```
export SERVICE_ACCOUNT=
```
```
export CLUSTER_NAME=
```

## Task 1: Create a custom security role.
```
gcloud config set compute/zone us-east1-b
```
```
wget filename
```

```
sed sed -i "s/<IAM-ROLE-NAME-TASK-1>/$CUSTOM_SECURIY_ROLE/g" role-definition.yaml
```

```
gcloud iam service-accounts create orca-private-cluster-sa --display-name "Orca Private Cluster Service Account"
gcloud iam roles create $CUSTOM_SECURIY_ROLE --project $DEVSHELL_PROJECT_ID --file role-definition.yaml

```

## Task 2: Create a service account

```
gcloud iam service-accounts create $SERVICE_ACCOUNT --display-name "Orca Private Cluster Service Account"
```

## Task 3: Bind a custom security role to an account

```
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/monitoring.viewer

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/monitoring.metricWriter

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/logging.logWriter

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:$SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role projects/$DEVSHELL_PROJECT_ID/roles/$CUSTOM_SECURIY_ROLE

```
## Task 4: Create and configure a new Kubernetes Engine private cluster
```
gcloud container clusters create $CLUSTER_NAME --num-nodes 1 --master-ipv4-cidr=172.16.0.64/28 --network orca-build-vpc --subnetwork orca-build-subnet --enable-master-authorized-networks  --master-authorized-networks 192.168.10.2/32 --enable-ip-alias --enable-private-nodes --enable-private-endpoint --service-account $SERVICE_ACCOUNT@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --zone us-east1-b

```








