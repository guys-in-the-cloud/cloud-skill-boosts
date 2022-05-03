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
gcloud dataproc clusters create sample-cluster
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
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- /data.txt
```
## Dataprep Job
- Initialize Dataprep
- Create flow
- Import gs://cloud-training/gsp323/runs.csv
- Create a Recipe
- Add the following Steps
 ![image](https://user-images.githubusercontent.com/104570014/166557351-1469d0e7-6a31-4919-a780-8074bb250653.png)


# Task 4: AI

- Execute The Following Commands

```
gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"

gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${YOUR_PROJECT}.iam.gserviceaccount.com

export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"

gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > result

gsutil cp result.json gs://$YOUR_PROJECT-marking/task4-cnl.result
```
- Copy the speech API configuration file 
```
wget https://raw.githubusercontent.com/guys-in-the-cloud/cloud-skill-boosts/main/Challenge-labs/Perform%20Foundational%20Data%2C%20ML%2C%20and%20AI%20Tasks%20in%20Google%20Cloud%3A%20Challenge%20Lab/speech-request.json
```
- Copy the Video Intelligence configuration file
```
waget https://github.com/guys-in-the-cloud/cloud-skill-boosts/blob/main/Challenge-labs/Perform%20Foundational%20Data%2C%20ML%2C%20and%20AI%20Tasks%20in%20Google%20Cloud:%20Challenge%20Lab/video-intelligence-request.json
```
