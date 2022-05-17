# Configure Secure RDP using a Windows Bastion Host

## Task 1: A new non-default VPC has been created
```
gcloud compute networks create securenetwork --subnet-mode custom
```
## Task 2: The new VPC contains a new non-default subnet within it
```
gcloud compute networks subnets create securenetwork-subnet --network=securenetwork --region us-central1 --range=192.168.16.0/20
```

## Task 3: A firewall rule exists that allows TCP port 3389 traffic ( for RDP )
```
gcloud compute firewall-rules create rdp-ingress-fw-rule --allow=tcp:3389 --source-ranges 0.0.0.0/0 --target-tags allow-rdp-traffic --network securenetwork

```
## Task 4: A Windows compute instance called vm-bastionhost exists that has a public ip-address to which the TCP port 3389 firewall rule applies.
```
gcloud compute instances create vm-bastionhost --zone=us-central1-a --machine-type=e2-medium --network-interface=subnet=securenetwork-subnet --network-interface=subnet=default,no-address --tags=allow-rdp-traffic --image=projects/windows-cloud/global/images/windows-server-2016-dc-v20220513
```

## Task 5: A Windows compute instance called vm-securehost exists that does not have a public ip-address
```
gcloud compute instances create vm-securehost --zone=us-central1-a --machine-type=e2-medium --network-interface=subnet=securenetwork-subnet,no-address --network-interface=subnet=default,no-address --tags=allow-rdp-traffic --image=projects/windows-cloud/global/images/windows-server-2016-dc-v20220513

```
## Task 6: The vm-securehost is running Microsoft IIS web server software.
```
gcloud compute reset-windows-password vm-bastionhost --user app_admin --zone us-central1-a
```
Now, copy the password for app_admin user, we need this to login into bastion vm 

# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
