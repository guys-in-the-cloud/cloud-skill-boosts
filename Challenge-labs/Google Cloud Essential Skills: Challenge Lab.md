# Google Cloud Essential Skills: Challenge Lab
```
export VM_NAME=
```
 example variable defination - export VM_NAME=<tag_given_in_the_lab_instructions>>
```
export ZONE= 
```
 example variable defination - export ZONE=<tag_given_in_the_lab_instructions>

## Create a Linux VM Instance
```
gcloud compute instances create $VM_NAME \
--zone=$ZONE \
--machine-type=e2-medium \
--tags=http-server,https-server \
--image=projects/debian-cloud/global/images/debian-10-buster-v20220406 \
--metadata=startup-script=\#\!\ /bin/bash$'\n'apt-get\ update$'\n'apt-get\ install\ apache2\ -y$'\n'service\ --status-all$'\n' 
  ```
