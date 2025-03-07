#### To create the Infrastructure for RKE Cluster #########
```
1. Provide the kms_key_id of your AWS Account to encrypt the EBS in terraform.tfvars.
2. Provide Public key in the file user_data_agent.sh, user_data_jumpbox.sh and user_data_server.sh.
```
In this section to create the RKE cluster I used a terraform script which is present in this GitHub Repo. The RKE cluster had three masters (RKE Servers) and three worker nodes (RKE Agents). To create the RKE Cluster using the Terraform I created a s3 bucket named as dexter-medermatherema and in this s3 bucket I kept the cluster.yaml (configuration file to create the RKE Cluster) and SSH private key to login into the RKE Servers and Agents. Finally, I created the RKE Cluster as shown in the screenshot attached below.

![image](https://github.com/user-attachments/assets/034d1611-5f9c-4599-8916-36e420b4fe89)
![image](https://github.com/user-attachments/assets/2798c190-9adb-4499-8dee-cc1d4c7b6d62)
 
As discussed above I kept the SSH private key in the s3 bucket, I created the SSH keys (Private and Public Key) using the command **ssh-keygen -t rsa -b 2048** as shown in the screenshot attached below. 
 
![image](https://github.com/user-attachments/assets/a0737ff0-d3e8-4d43-971d-daa64d5d5b93)
