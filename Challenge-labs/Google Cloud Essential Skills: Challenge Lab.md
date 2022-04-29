```
export VM_NAME=
```
```
export ZONE= 
```
```
gcloud compute instances create $VM_NAME \
--zone=$ZONE \
--machine-type=e2-medium \
--tags=http-server,https-server \
--image=projects/debian-cloud/global/images/debian-10-buster-v20220406 \
--metadata=startup-script=\#\!\ /bin/bash$'\n'apt-get\ update$'\n'apt-get\ install\ apache2\ -y$'\n'service\ --status-all$'\n' 
  ```
