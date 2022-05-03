# Serverless Cloud Run Development: Challenge Lab

[YouTube Video Link](https://youtu.be/NMI4KZuBWMc)

## Let's start with defining some variables given by Cloud Skill Boosts

```
export PUBLIC_BILLING_SERVICE=
```
```
export FRONTEND_STAGING_SERVICE=
```
```
export PRIVATE_BILLING_SERVICE=
```
```
export BILLING_SERVICE=
```
```
export BILLING_PROD_SERVICE=
```
```
export FRONTEND_SERVICE=
```
```
gcloud config set run/region us-central1
gcloud config set run/platform managed

git clone https://github.com/rosera/pet-theory.git && cd pet-theory/lab07

```
## Task - 1 : Deploy a Public Billing Service

```
cd ~/pet-theory/lab07/unit-api-billing

gcloud builds submit --tag gcr.io/$$DEVSHELL_PROJECT_ID/billing-staging-api:0.1
gcloud run deploy $PUBLIC_BILLING_SERVICE --image gcr.io/$DEVSHELL_PROJECT_ID/billing-staging-api:0.1

```
## Task - 2 : Deploy the Frontend Service

```
cd ~/pet-theory/lab07/staging-frontend-billing

gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/frontend-staging:0.1
gcloud run deploy $FRONTEND_STAGING_SERVICE --image gcr.io/$DEVSHELL_PROJECT_ID/frontend-staging:0.1

gcloud run services list

```
## Task - 3 : Deploy a Private Billing Service
```
cd ~/pet-theory/lab07/staging-api-billing

gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/billing-staging-api:0.2
gcloud run deploy $PRIVATE_BILLING_SERVICE --image gcr.io/$DEVSHELL_PROJECT_ID/billing-staging-api:0.2


```


## Task - 4 : Create a Billing Service Account

```
gcloud iam service-accounts create $BILLING_SERVICE --display-name "Billing Service Cloud Run"
```

## Task - 5 : Deploy a Billing Service in Production
```
cd ~/pet-theory/lab07/prod-api-billing

gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/billing-prod-api:0.1
gcloud run deploy $BILLING_PROD_SERVICE --image gcr.io/$DEVSHELL_PROJECT_ID/billing-prod-api:0.1


```
## Task - 6 : Create a Frontend Service Account
```
gcloud iam service-accounts create $FRONTEND_SERVICE --display-name "Billing Service Cloud Run Invoker"
```
## Task - 7 : Deploy the Frontend Service in Production
```
cd ~/pet-theory/lab07/prod-frontend-billing

gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/frontend-prod:0.1
gcloud run deploy public-billing-service --image gcr.io/$DEVSHELL_PROJECT_ID/frontend-prod:0.1

gcloud run services list

```



