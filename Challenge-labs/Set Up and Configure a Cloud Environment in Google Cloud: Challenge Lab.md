# Implement DevOps in Google Cloud: Challenge Lab
[YouTube Video Link](https://youtu.be/cHfKWcQglbo)

## Let's start with defining some variables given by Cloud Skill Boosts
```
export COLOUR=
```

```
export VERSION=
```

## Task1 Check Jenkins pipeline has been configured
```
gcloud config set compute/zone us-east1-b
```
```
git clone https://source.developers.google.com/p/$DEVSHELL_PROJECT_ID/r/sample-app
gcloud container clusters get-credentials jenkins-cd
```
```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=$(gcloud config get-value account)
helm repo add stable https://kubernetes-charts.storage.googleapis.com/  → this might not work… use the next line of code instead…
helm repo add stable https://charts.helm.sh/stable
helm repo update
helm install cd stable/jenkins
kubectl get pods
```
```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

- On google cloud console --> Web preview --> Preview on port 8080 --> Add username as `admin` and password obtained on the last line of the ouput from previous command --> configure Credential--> Manage jenkins--> Manage credentials-->global --> adding some credentials? -->Google Service Account from metadata--> project name --> Ok
- Create Multibranch pipeline of sample-app 
  - Dashboard --> new item --> enter name as `sample-app` -> Select `Multibranch pipeline` 
  - `Branch Sources` tab -> Add source --> Select `Git`
  - Complete Task 2 meanwhile
- Project Repository: https://source.developers.google.com/p/[PROJECT_ID]/r/sample-app
- Credentials: qwiklabs service account
- `Scan multibranch Pipeline Triggers`
  - Check `Periodically if not otherwise run`--> Interval `1 min`

## Task2 Check that Jenkins has deployed a development pipeline
```
cd sample-app
kubectl create ns production
kubectl apply -f k8s/production -n production
kubectl apply -f k8s/canary -n production
kubectl apply -f k8s/services -n production
```
```
sed -i "s/blue/$COLOUR/g" html.go
```
```
sed -i "s/1.0.0/$VERSION/g" main.go
```
```
kubectl get svc
kubectl get service gceme-frontend -n production
```


## Task3 Check that Jenkins has deployed a canary pipeline
```
git init
git config credential.helper gcloud.sh
git remote add origin https://source.developers.google.com/p/$DEVSHELL_PROJECT_ID/r/sample-app
```
```
git config --global user.email "<user email>"
git config --global user.name "<user name>"
```
```
git add .
git commit -m "initial commit"
git push origin master
```
## Task4 Check that Jenkins has merged a canary pipeline with production
```
git checkout -b new-feature
```
```
git add Jenkinsfile html.go main.go
git commit -m "Version 2.0.0"
git push origin new-feature
```
```
curl http://localhost:8001/api/v1/namespaces/new-feature/services/gceme-frontend:80/proxy/version
kubectl get service gceme-frontend -n production
```
```
git checkout -b canary
git push origin canary
export FRONTEND_SERVICE_IP=$(kubectl get -o \
jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
git checkout master
git push origin master
```
```
export FRONTEND_SERVICE_IP=$(kubectl get -o \
jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
while true; do curl http://$FRONTEND_SERVICE_IP/version; sleep 1; done
```
Error messages in console like `curl: (7) Failed to connect to 34.74.152.38 port 80: Connection refused`
- So go to jenkins tab --> Click on `sample-app` in navbar
- Check if any build in the name column is red colored and not with green check
- If red then, click on it, on the left panel -> `Build Now`
- Wait for it to build, it'll take some time
- Go back to `sample-app` home page and see if it got green. 
```
kubectl get service gceme-frontend -n production
```
```
git merge canary
git push origin master
export FRONTEND_SERVICE_IP=$(kubectl get -o \
jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
```

# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
