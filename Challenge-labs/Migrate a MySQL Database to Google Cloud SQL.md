# Migrate a MySQL Database to Google Cloud SQL

[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)


## Task 1: Check that there is a Cloud SQL instance

gcloud sql instances create wordpress --tier=db-n1-standard-1 --activation-policy=ALWAYS --gce-zone=us-central1-a --database-version=MYSQL_5_7

gcloud sql users set-password --host % root --instance wordpress --password Password1*

export BLOG_VM_EXTERNAL_IP=gcloud compute instances describe blog-vm \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)'

gcloud sql instances patch wordpress --authorized-networks $BLOG_VM_EXTERNAL_IP --quiet

gcloud compute ssh blog --zone=us-central1-a

export CLOUD_SQL_IP=$(gcloud sql instances describe Wordpress --project $DEVSHELL_PROJECT_ID --format 'value(ipAddresses.ipAddress)')

mysql --host=$CLOUD_SQL_IP --user=root --password=Password1*

CREATE DATABASE wordpress; CREATE USER 'blogadmin'@'%' IDENTIFIED BY 'Password1*'; GRANT ALL PRIVILEGES ON wordpress.* TO 'blogadmin'@'%'; FLUSH PRIVILEGES;

exit

sudo mysqldump -u root -pPassword1* wordpress > wordpress_db_backup.sql

mysql --host=$CLOUD_SQL_IP --user=root -pPassword1* --verbose wordpress < wordpress_db_backup.sql

sudo service apache2 restart

cd /var/www/html/wordpress

sudo nano wp-config.php

## Task 2 : Check that there is a user database on the Cloud SQL instance
```
gsutil cp -r gs://cloud-training/gsp321/dm .

cd dm

sed -i s/SET_REGION/us-east1/g prod-network.yaml

gcloud deployment-manager deployments create prod-network \
    --config=prod-network.yaml

cd ..
```
## Task 3 : Check that the blog instance is authorized to access Cloud SQL
```
gcloud compute instances create bastion --network-interface=network=griffin-dev-vpc,subnet=griffin-dev-mgmt  --network-interface=network=griffin-prod-vpc,subnet=griffin-prod-mgmt --tags=ssh --zone=us-east1-b

gcloud compute firewall-rules create fw-ssh-dev --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-dev-vpc

gcloud compute firewall-rules create fw-ssh-prod --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-prod-vpc

```


## Task 4 : Check that wp-config.php points to the Cloud SQL instance
```
gcloud sql instances create griffin-dev-db --root-password password --region=us-east1 --database-version=MYSQL_5_7

gcloud sql connect griffin-dev-db

CREATE DATABASE wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO "wp_user"@"%" IDENTIFIED BY "stormwind_rules";
FLUSH PRIVILEGES;

exit

```

## Task 5 : Check that the blog still responds to requests

```
gcloud container clusters create griffin-dev \
  --network griffin-dev-vpc \
  --subnetwork griffin-dev-wp \
  --machine-type n1-standard-4 \
  --num-nodes 2  \
  --zone us-east1-b


gcloud container clusters get-credentials griffin-dev --zone us-east1-b

cd ~/

gsutil cp -r gs://cloud-training/gsp321/wp-k8s .

```


# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
