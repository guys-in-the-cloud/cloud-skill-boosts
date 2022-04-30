# Perform Foundational Infrastructure Tasks in Google Cloud: Challenge Lab

[YouTube Video Link](https://youtu.be/ZdZ3SiarZrs)

## Let's start with defining some variables given by Cloud Skill Boosts

Defining Cloud Function name variable
```
export CLOUDFUNCTION_NAME=
```
Defining entrypoint variable
```
export ENTRYPOINT=
```

## Downloading main.py and requirements.txt file we will use this while creating cloud function 

```
wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Build%20Interactive%20Apps%20with%20Google%20Assistant:%20Challenge%20Lab/main.py

wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Build%20Interactive%20Apps%20with%20Google%20Assistant:%20Challenge%20Lab/requirements.txt

```

## Task 1: Create the Cloud Function for the Magic Eight Ball app

1.1 Replacing the variable with REPLACE_WITH_YOUR_ENTRY_POINT
```
sed -i "s/REPLACE_WITH_YOUR_ENTRY_POINT/$ENTRYPOINT/g" main.py
```

1.2 Deploy the function 
```
gcloud functions deploy $CLOUDFUNCTION_NAME \
    --entry-point $ENTRYPOINT \
    --runtime python39 \
    --max-instances 5 \
    --trigger-http --allow-unauthenticated \
    --region us-central1
```

## Task 2: Create the Magic 8 Ball app for Google Assistant



## Task 3: Add multilingual support to your Cloud Function



# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud..

