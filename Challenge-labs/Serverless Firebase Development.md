# Serverless Firebase Development

[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)

## Let's start with defining some variables given by Cloud Skill Boosts

```
export DATASET_SERVICE_NAME=
```
```
export FRONTEND_STAGING_SERVICE_NAME=
```
```
export FRONTEND_PRODUCTION_SERVICE_NAME=
```
# Prerequistes 

Clone the official git repo for this challenge lab 
```
git clone https://github.com/rosera/pet-theory.git
```

# Task 1: Create a Firestore database
database

# Task 2: Populate the Database
```
cd pet-theory/lab06/firebase-import-csv/solution
npm install
node index.js netflix_titles_original.csv
```

# Task 3: Create a REST API
```
cd ~/pet-theory/lab06/firebase-rest-api/solution-01
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1
gcloud beta run deploy $DATASET_SERVICE_NAME --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1 --allow-unauthenticated
```
# Task 4: Firestore API access
```
cd ~/pet-theory/lab06/firebase-rest-api/solution-02
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2
gcloud beta run deploy $DATASET_SERVICE_NAME --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2 --allow-unauthenticated

```
- Goto cloud run and click netflix-dataset-service then copy the url.
```
SERVICE_URL=<copy url from your netflix-dataset-service>
curl -X GET $SERVICE_URL/2019
```

# Task - 5: Cloud Build Frontend Staging
```
cd ~/pet-theory/lab06/firebase-frontend/public
nano app.js # comment line 3 and uncomment line 4, insert your netflix-dataset-service url
npm install
cd ~/pet-theory/lab06/firebase-frontend
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
gcloud beta run deploy $FRONTEND_STAGING_SERVICE_NAME --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
```
- Choose 1 and us-central1
# Task - 6: Cloud Build Frontend Production
```
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
gcloud beta run deploy $FRONTEND_PRODUCTION_SERVICE_NAME --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
```


