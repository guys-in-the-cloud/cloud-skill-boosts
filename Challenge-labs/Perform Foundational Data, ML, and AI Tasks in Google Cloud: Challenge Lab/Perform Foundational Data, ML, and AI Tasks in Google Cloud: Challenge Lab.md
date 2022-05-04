## Perform Foundational Data, ML, and AI Tasks in Google Cloud: Challenge Lab
[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)

## Defining some variables given by Cloud Skill Boosts

```
export BIGQUERY_DATASET_NAME=
```

```
export CLOUD_STORAGE_BUCKET_NAME=
```
```
export REGION=
```
# Task 1: Run a simple Dataflow job

- Set the zone by using command
```
gcloud config set compute/zone $REGION
```
- create Bigquery Dataset using command
```
bq mk $BIGQUERY_DATASET_NAME
```

```
gsutil mb gs://$CLOUD_STORAGE_BUCKET_NAME
```
# Task 2: Run a simple Dataproc job
![image](https://user-images.githubusercontent.com/104570014/166552910-15c708b1-68a3-4b5b-bf32-af1e174472ec.png)


# Task 3: Run a simple Dataprep job
- Set Region
```
gcloud config set compute/zone ${REGION}-a
```
```
gcloud dataproc clusters create sample-cluster --region ${REGION}
```
- SSH into Dataproc Cluster
```
gcloud compute ssh sample-cluster-m --zone=${REGION}-a
```

```
hdfs dfs -cp gs://cloud-training/gsp323/data.txt /data.txt
```
- exit the SSH terminal 
- Submit the Spark Dataproc Job
```
gcloud dataproc jobs submit spark --cluster sample-cluster \
  --class org.apache.spark.examples.SparkPageRank \
  --region $REGION \
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- /data.txt
```
## Dataprep Job
- Initialize Dataprep
- Create flow
- Import to GCS  gs://cloud-training/gsp323/runs.csv
- Edit a Recipe
- Add the following Steps
 ![image](https://user-images.githubusercontent.com/104570014/166557351-1469d0e7-6a31-4919-a780-8074bb250653.png)

- Follow this image given below <br>
![image](https://user-images.githubusercontent.com/104570014/166623601-47373082-c027-4407-8ff1-472d7b1bc28a.png)

# Task 4: AI

- Execute The Following Commands

```
gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"

gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com
```
## 4.1  Google Cloud Speech API 
```
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"

wget https://raw.githubusercontent.com/guys-in-the-cloud/cloud-skill-boosts/main/Challenge-labs/Perform%20Foundational%20Data%2C%20ML%2C%20and%20AI%20Tasks%20in%20Google%20Cloud%3A%20Challenge%20Lab/speech-request.json

curl -s -X POST -H "Content-Type: application/json" --data-binary @speech-request.json \ 
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > speech.json
```
```
gsutil cp speech.json gs://$DEVSHELL_PROJECT_ID-marking/<changefilename>
```
## 4.2 Cloud Natural Language API
```
gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > language.json
```
```
gsutil cp language.json gs://$DEVSHELL_PROJECT_ID-marking/<changefilename>
```

# 4.3 Google Video Intelligence

- Copy the Video Intelligence configuration file
```
wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/blob/main/Challenge-labs/Perform%20Foundational%20Data%2C%20ML%2C%20and%20AI%20Tasks%20in%20Google%20Cloud:%20Challenge%20Lab/video-intelligence-request.json
```
```
curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @video-intelligence-request.json  > video.json
```
```
gsutil cp video.json gs://$DEVSHELL_PROJECT_ID-marking/<changefilename>
``` 


