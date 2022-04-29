```
export VM_NAME=
```
```
export Zone= 
```
```
gcloud compute instances create $VM_NAME \
  --zone=$Zone
  --image-project=debian-cloud \
  --image-family=debian-10 \
  --metadata=startup-script='#! /bin/bash \
  --network-interface=network-tier=PREMIUM \
  --tags=http-server,https-server
  apt update
  apt -y install apache2
  cat <<EOF > /var/www/html/index.html
  <html><body><p>Linux startup script added directly.</p></body></html>'
  ```
