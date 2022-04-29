```
export VM_NAME=
```
```
export ZONE= 
```
Create a firewall rule to allow traffic on port 80 for last check
```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags http-server --network default
```

```
gcloud compute instances create $VM_NAME \
--zone=$ZONE \
--machine-type=e2-medium \
--tags=http-server,https-server \
--image=projects/debian-cloud/global/images/debian-10-buster-v20220406 \
--metadata=startup-script=\#\!\ /bin/bash$'\n'apt-get\ update$'\n'apt-get\ install\ apache2\ -y$'\n'service\ --status-all$'\n' 
  ```
