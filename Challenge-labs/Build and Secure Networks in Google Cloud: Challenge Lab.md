Task 1 : Remove the overly permissive rules

gcloud compute firewall-rules delete open-access

Task 2 : Start the bastion host instance

Task 3 : Create a firewall rule that allows SSH (tcp/22) from the IAP service and add network tag on bastion

gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags [NETWORK TAG-1] --network acme-vpc

gcloud compute instances add-tags bastion --tags=[NETWORK TAG-1] --zone=us-central1-b

Task 4 : Create a firewall rule that allows traffic on HTTP (tcp/80) to any address and add network tag on juice-shop

gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags [NETWORK TAG-2] --network acme-vpc

gcloud compute instances add-tags juice-shop --tags=[NETWORK TAG-2] --zone=us-central1-b

Task 5 : Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet network address and add network tag on juice-shop
gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192[dot]168[dot]10[dot]0/24 --target-tags [NETWORK TAG-3] --network acme-vpc

gcloud compute instances add-tags juice-shop --tags=[NETWORK TAG-3] --zone=us-central1-b

Task 6 : SSH to bastion host via IAP and juice-shop via bastion
ssh [Internal IP address of juice-shop]
