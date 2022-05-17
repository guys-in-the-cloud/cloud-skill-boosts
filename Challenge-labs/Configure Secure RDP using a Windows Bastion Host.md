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

## Manaul steps
- Click on Add roles and features 
![image](https://user-images.githubusercontent.com/104570014/168892931-33d20ae2-c0c5-424b-af2f-f73e5ae78368.png)
![image](https://user-images.githubusercontent.com/104570014/168893306-de8312ea-8d42-475c-b93b-fd06e32387c6.png)
![image](https://user-images.githubusercontent.com/104570014/168893438-8c8968d9-08b3-4f92-b13e-6ef8c7a7a6e6.png)
![image](https://user-images.githubusercontent.com/104570014/168893562-601a4d6a-710a-4124-91fd-5176cfad5bd1.png)
![image](https://user-images.githubusercontent.com/104570014/168893704-1500a6fa-0a29-4926-be2b-35078a8e0a12.png)
- Click next 
- Click on install 




# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...
