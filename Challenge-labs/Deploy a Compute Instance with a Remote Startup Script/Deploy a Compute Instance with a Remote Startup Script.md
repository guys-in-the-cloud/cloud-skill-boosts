# Build and Secure Networks in Google Cloud - Challenge Lab

[YouTube Video Link](https://youtu.be/ZdZ3SiarZrs)

## Task 1: Confirm that a Google Cloud Storage bucket exists that contains a file

1.1 Creating a fine-grained type of bucket
```
gsutil mb -b off gs://$DEVSHELL_PROJECT_ID
```
1.2 Downloading a sample script given in the lab instructions
```
wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Deploy%20a%20Compute%20Instance%20with%20a%20Remote%20Startup%20Script/resources-install-web.sh
```
1.3 Uploading downloaded script file to the bucket
```
gsutil cp resources-install-web.sh gs://$DEVSHELL_PROJECT_ID
```
## Task 2: Confirm that a compute instance has been created that has a remote startup script called install-web.sh configured
``` 
gcloud compute instances add-metadata lab-monitor --metadata startup-script-url=gs://$DEVSHELL_PROJECT_ID/resources-install-web.sh --zone us-central1-a
```

## Task 3: Confirm that a HTTP access firewall rule exists with tag that applies to that virtual machine

3.1 Create a firewall rule to allow traffic on port 80
```
gcloud compute firewall-rules create http-fw-rule --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags allow-http-traffic --network default
```
3.2 Update the previously created VM & add network tag to the VM to allow http traffic
```
gcloud compute instances add-tags lab-monitor --tags=allow-http-traffic --zone=us-central1-a
```
3.3 Reset the lab-monitor vm so that our script will be applied 
```
gcloud compute instances reset lab-monitor --zone us-central1-a
```
## Task 4: Connect to the server ip-address using HTTP and get a non-error response

4.1 Exporting external IP of the VM to a variable
```
export VM_EXTERNAL_IP=${gcloud compute instances describe lab-monitor \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)'}
```
Now, let's validate that we've installed a webserver successfully & it is serving on port 80
```
curl http://$VM_EXTERNAL_IP
```

# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
