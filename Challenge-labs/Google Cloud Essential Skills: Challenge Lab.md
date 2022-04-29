# Google Cloud Essential Skills: Challenge Lab
[YouTube Video Link](https://youtu.be/ZdZ3SiarZrs)


## Let's start with defining some variables given by Cloud Skill Boosts

```
export VM_NAME=
```
 example variable defination - export VM_NAME=<tag_given_in_the_lab_instructions>>
```
export ZONE= 
```
example variable defination - export ZONE=<tag_given_in_the_lab_instructions>

## Task1 Create a Compute Engine instance, add necessary firewall rules.
```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags http-server --network default
```
## Task2 Configure Apache2 Web Server in your instance
```
gcloud compute instances create $VM_NAME \
--zone=$ZONE \
--machine-type=e2-medium \
--tags=http-server,https-server \
--image=projects/debian-cloud/global/images/debian-10-buster-v20220406 \
--metadata=startup-script=\#\!\ /bin/bash$'\n'apt-get\ update$'\n'apt-get\ install\ apache2\ -y$'\n'service\ --status-all$'\n' 
  ```
## Task3 Test your server
```
gcloud compute instances describe $VM_NAME \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)'
```

