# Migrate a MySQL Database to Google Cloud SQL

[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)


## Task 1: Check that there is a Cloud SQL instance

1.1 Creating a cloudSQL instance named wordpress
```
gcloud sql instances create wordpress --tier=db-n1-standard-1 --activation-policy=ALWAYS --zone=us-central1-a --database-version=MYSQL_5_7
```
1.2 Setting up default root user & their password
```
gcloud sql users set-password --host % root --instance wordpress --password Password1*
```

## Task 2 & 3: Check that there is a user database on the Cloud SQL instance & Check that the blog instance is authorized to access Cloud SQL

1. Storing blog vm external IP into a variable
```
export BLOG_VM_EXTERNAL_IP=$(gcloud compute instances describe blog --zone=us-central1-a --format='get(networkInterfaces[0].accessConfigs[0].natIP)')
```
2. Authorize blog VM to your cloudSQL instance
```
gcloud sql instances patch wordpress --authorized-networks $BLOG_VM_EXTERNAL_IP/32 --quiet
```
3. Connect to blog VM using SSH
```
gcloud compute ssh blog --zone=us-central1-a
```
4. Storing CloudSQL instance IP into a variable
```
export CLOUD_SQL_IP=$(gcloud sql instances describe wordpress --format 'value(ipAddresses.ipAddress)')
```
5. Connecting to the cloudSQL instance
```
mysql --host=$CLOUD_SQL_IP --user=root --password=Password1*
```
6. Creating database & user for your cloud SQL database
```
CREATE DATABASE wordpress; CREATE USER 'blogadmin'@'%' IDENTIFIED BY 'Password1*'; GRANT ALL PRIVILEGES ON wordpress.* TO 'blogadmin'@'%'; FLUSH PRIVILEGES;
```
all done from here, let's exit
```
exit
```
7. take dump of your cloud SQL instance
```
sudo mysqldump -u root -pPassword1* wordpress > wordpress_db_backup.sql
```
8. Restore the dump 
```
mysql --host=$CLOUD_SQL_IP --user=root -pPassword1* --verbose wordpress < wordpress_db_backup.sql
```

## Task 4 & 5: Check that wp-config.php points to the Cloud SQL instance and Check that the blog still responds to requests

1. Restart the Apache webserver service
```
sudo service apache2 restart
```
2. Replace the Database IP configuration from localhost to our cloudSQL instance IP
```
sudo sed -i "s/localhost/$CLOUD_SQL_IP/g" /var/www/html/wordpress/wp-config.php
```


# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
