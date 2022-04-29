## Check Jenkins pipeline has been configured
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
## Check that Jenkins has deployed a development pipeline
```
cd sample-app
kubectl create ns production
kubectl apply -f k8s/production -n production
kubectl apply -f k8s/canary -n production
kubectl apply -f k8s/services -n production
```
```
kubectl get svc
kubectl get service gceme-frontend -n production
```
Branch Sources: Git
- Project Repository: https://source.developers.google.com/p/[PROJECT_ID]/r/sample-app
- Credentials: qwiklabs service account
