# Deploy to Kubernetes in Google Cloud

[YouTube Video Link]()


## Defining some variables given by Cloud Skill Boosts

```
export DOCKER_IMAGE=
```
example variable defination - export Docker_Image=<Docker Image>

```
export DOCKER_IMAGE_UPDATED_VERSION=
```
example variable defination - export Updated Version=<Updated Version>

  ```
  export Your_email=
  ```
example variable defination -  export Your_email= <you@example.com>
  
  ```
  export User_name=
  ```
example variable defination - export User_name= <student5674..>
  
  
## Task 1: Create a Docker image and store the Dockerfile
  ```
  gcloud auth list
  gsutil cat gs://cloud-training/gsp318/marking/setup_marking_v2.sh | bash
  gcloud source repos clone valkyrie-app
  cd valkyrie-app
 ```
The app source code is in valkyrie-app/source. Create valkyrie-app/Dockerfile and add the configuration below.  
```
cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF
```
 Building image  
```
docker build -t ${DOCKER_IMAGE} .
```
```
cd ..
cd marking
./step1_v2.sh
```
  Task - 3 : Push the Docker image in the Google Container Repository
```
cd ..
cd valkyrie-app
docker tag $Docker_Image:$Tag Name gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE}
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE}
```
Task - 4 : Create and expose a deployment in Kubernetes
```
sed -i s#IMAGE_HERE#gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE} Name#g k8s/deployment.yaml
gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
kubectl create -f k8s/deployment.yaml
kubectl create -f k8s/service.yaml
```
  Task - 5 : Update the deployment with a new version of valkyrie-app
 ```
  git merge origin/kurt-dev
  kubectl edit deployment valkyrie-dev
  
  ## Edit Replicas from 1 to replicas given in lab manual 1 to <Replicas Count>
  
  ## ### change <Tag Name> to <Updated Version> in two places
  
  ```
  docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE_UPDATED_VERSION}.
  docker push gcr.io/$GOOGLE_CLOUD_PROJECT/${DOCKER_IMAGE_UPDATED_VERSION}
  ```
  
  Task - 6 : Create a pipeline in Jenkins to deploy your app
  
  ```
  docker ps
  docker kill <container id>
  ```
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
 ```
  Go through the following:

-> Manage Jenkins -> Manage Credentials -> Jenkins -> Global credentials (unrestricted) -> Add credentials -> Kind: Google Service Account from metadata -> OK

-> Jenkins -> New Item -> Name : valkyrie-app -> Pipeline -> Pipeline script from SCM -> Set SCM to git -> OK

-> Pipeline -> Script: Pipeline script from SCM -> SCM: Git

-> Repository URL: {find it using command: gcloud source repos list} -> Credentials: {Project id}

-> Apply -> Save
  
  ```
  sed -i "s/green/orange/g" source/html.go
sed -i "s/YOUR_PROJECT/$GOOGLE_CLOUD_PROJECT/g" Jenkinsfile
git config --global user.email "you@example.com"              // Email
git config --global user.name "student..."                       // Username
git add .
git commit -m "built pipeline init"
git push
  ```
