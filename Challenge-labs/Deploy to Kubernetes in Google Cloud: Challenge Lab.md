# Deploy to Kubernetes in Google Cloud

[YouTube Video Link](https://youtu.be/y7buagD2urw)


## Defining some variables given by Cloud Skill Boosts

```
export YOUR_EMAIL=
```
example variable defination -  export Your_email= <you@example.com>
  
```
export USERNAME=
```
example variable defination - export User_name= <student5674..>

```
export DOCKER_IMAGE=
```
example variable defination - export Docker_Image=<Docker Image>

```
export TAG_NAME=
```
  
```
export UPDATED_VERSION=
```
example variable defination - export Updated Version=<Updated Version>

```
export REPLICAS_COUNT=
```
  
  
## Task 1: Create a Docker image and store the Dockerfile

1.1 clone the lab repo 
```
gsutil cat gs://cloud-training/gsp318/marking/setup_marking_v2.sh | bash
gcloud source repos clone valkyrie-app
cd valkyrie-app
```

1.2 creating a dockerfile to create docker image
```
cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF
```
  
1.3 Building image  
```
docker build -t ${DOCKER_IMAGE}:${TAG_NAME} .
```
  
1.4 verify the docker image existence for check point by running the step1_v2.sh script file
```
cd ..
cd marking
./step1_v2.sh
```
Image exists<br>
Go ahead and check the activity tracking on the lab page
  
1.5 run the created docker conatiner image and map this to port number 8080 with the host 
  
```
docker run -dit -p 8080:8080 ${DOCKER_IMAGE}:${TAG_NAME}
```
  
1.6 verify the docker image running status for check point by running the step2_v2.sh script file
  
```
./step2_v2.sh 
```
you should see this output 

Container running and visible on port 8080, good job! <br>
Go ahead and check the activity tracking on the lab page

## Task 3: Push the Docker image in the Google Container Repository
  
```
cd ..
cd valkyrie-app
docker tag ${DOCKER_IMAGE}:${TAG_NAME} gcr.io/$DEVSHELL_PROJECT_ID/${DOCKER_IMAGE}:${TAG_NAME}
docker push gcr.io/$DEVSHELL_PROJECT_ID/${DOCKER_IMAGE}:${TAG_NAME}
```
## Task 4 : Create and expose your application
```
sed -i s#IMAGE_HERE#gcr.io/$DEVSHELL_PROJECT_ID/${DOCKER_IMAGE}:${TAG_NAME}#g k8s/deployment.yaml
gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
kubectl create -f k8s/deployment.yaml
kubectl create -f k8s/service.yaml
```
  
## Task 5 : Update the deployment with a new version of valkyrie-app

5.1 Merge the branch & change the no. of replicas
  
```
git merge origin/kurt-dev
sed -i "s/replicas: 1/replicas: $REPLICAS_COUNT/g" k8s/deployment.yaml
kubectl apply -f k8s/deployment.yaml
```
5.2 Build & Push the container image with image  
 
```
docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE}:${UPDATED_VERSION} .
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE}:${UPDATED_VERSION}
```
5.3 Deploy the new image to your kubernetes application
  
```
sed -i s#${TAG_NAME}#${UPDATED_VERSION}#g k8s/deployment.yaml
kubectl apply -f k8s/deployment.yaml
```
  
## Task 6: Create a pipeline in Jenkins to deploy your app
  
6.1 Jenkins runs on 8080 & we previously ran first image on the same port so first remove it
 
```
docker rm -f $(docker ps -aq)
```
6.2 Let's deploy the jenkins pod in your cluster  
  
```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```
Note - In the output of the last command give the password for your jenkins admin user, keep this while login to the Jenkins.
  
## Manual Things
  
1. In the cloudshell -> click on web preview on port 8080 -> type user as admin & password from your last cloud shell output

2. Manage Jenkins -> Manage Credentials -> Jenkins -> Global credentials (unrestricted) -> Add credentials -> Kind: Google Service Account from metadata    -> done

Go back to the cloud shell & run the following command to get your repo URL 

```
gcloud source repos list
```

Go back to the jenkins console & create a pipeline
  
3. Dashboard -> New Item -> Give name as <b>valkyrie-app</b> -> in the Pipeline section -> choose Pipeline script from SCM from the dropdown -> Script:            Pipeline script from SCM -> SCM: Git ->  Repository URL -> fill your project ID as Credentials -> Apply -> Save


Again go back to the cloud shell & run the following command, to test your pipeline
  
```
sed -i "s/green/orange/g" source/html.go
sed -i "s/YOUR_PROJECT/$GOOGLE_CLOUD_PROJECT/g" Jenkinsfile
git config --global user.email "${YOUR_EMAIL}"              
git config --global user.name "${USERNAME}"                 
git add .
git commit -m "built pipeline init"
git push
```
## In the Jenkins console output if you see All nodes of label 'valkyrie-app' are offline then <br>
  first delete the existing 'valkyrie-app' pipeline: in the left hand side > delete pipeline > repeat the process from Manual thing section <b>3rd point</b>  
# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
