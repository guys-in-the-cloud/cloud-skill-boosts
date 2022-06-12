# Build and Secure Networks in Google Cloud - Challenge Lab

[YouTube Video Link](https://youtu.be/ZdZ3SiarZrs)

## Let's start with defining some variables given by Cloud Skill Boosts

```
export SSH_IAP_NETWORK_TAG=
```
example variable defination - export SSH_IAP_NETWORK_TAG=<SSH_IAP_network_tag_given_in_the_lab_instructions> 
```
export SSH_INTERNAL_NETWORK_TAG=
```
example variable defination - export SSH_INTERNAL_NETWORK_TAG=<SSH_internal_network_tag_given_in_the_lab_instructions>
  
```
export HTTP_NETWORK_TAG=
```
example variable defination - export HTTP_NETWORK_TAG=<HTTP_network_tag_given_in_the_lab_instructions>



## Task 1 : Remove the overly permissive rules
```
gcloud compute firewall-rules delete open-access
```

## Task 2 : Start the bastion host instance
```
gcloud compute instances start bastion --zone=us-central1-b
```

## Task 3 : Create a firewall rule that allows SSH (tcp/22) from the IAP service and add network tag on bastion

1. Create a IAP firewall rule with specific tag
```
gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags $SSH_IAP_NETWORK_TAG --network acme-vpc
```
2. Attach firewall tag to the bastion VM
```
gcloud compute instances add-tags bastion --tags=$SSH_IAP_NETWORK_TAG --zone=us-central1-b
```

## Task 4 : Create a firewall rule that allows traffic on HTTP (tcp/80) to any address and add network tag on juice-shop

1. Create a HTTP firewall rule with specific tag
```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags $HTTP_NETWORK_TAG --network acme-vpc
```
2. Attach firewall tag to the juice-shop VM
```
gcloud compute instances add-tags juice-shop --tags=$HTTP_NETWORK_TAG --zone=us-central1-b
```

## Task 5 : Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet network address and add network tag on juice-shop

1. Create a Internal SSH firewall rule with specific tag
```
gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags $SSH_INTERNAL_NETWORK_TAG --network acme-vpc
```
2. Attach firewall tag to the juice-shop VM
```
gcloud compute instances add-tags juice-shop --tags=$SSH_INTERNAL_NETWORK_TAG --zone=us-central1-b
```

## Task 6 : SSH to bastion host via IAP and juice-shop via bastion
```
gcloud compute ssh --zone "us-central1-b" "bastion"  --tunnel-through-iap --project $DEVSHELL_PROJECT_ID
```
If prompted, please type yes & then enter two times. You'll see you're successfully login to the bastion VM

Now, Validate our Internal SSH firewall rule working file, so for that let's SSH to juice-shop VM from bastion.

1. Exporting internal IP of the juice-shop VM
```
export JUICE_SHOP_VM_INTERNAL_IP=$(gcloud compute instances describe juice-shop --zone=us-central1-b --format='get(networkInterfaces[0].networkIP)')
```
SSH to the juice-shop VM
```
gcloud compute ssh juice-shop --internal-ip
```
If prompted, please type yes & then enter two times. You'll see you're successfully login to the juice-shop VM from bastion VM, It means our SSH
firewall is working perfectly

If you get <b> Public key access denied</b> 
Use this command:-
```
ssh $JUICE_SHOP_VM_INTERNAL_IP
```

SSH to bastion host via IAP and juice-shop via bastion


# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
