#### To create the Infrastructure for K3S Cluster #########

1. Provide the kms_key_id of your AWS Account to encrypt the EBS in terraform.tfvars.
2. Provide the monitoring_role_arn in the file terraform.tfvars.
3. Provide the kms_key_id_rds in the file terraform.tfvars.
4. Provide Public key in user_data.sh

### K3S HA Cluster with etcd as cluster datastore 
```
curl -sfL https://get.k3s.io | sh -s - server --cluster-init --tls-san=network-loadbalancer-k3s-XXXXXXXXXXXXXXXX.elb.us-east-2.amazonaws.com:6443   -----> On First Master

cat /var/lib/rancher/k3s/server/node-token   --------------> Get the K3S Token from this command

curl -sfL https://get.k3s.io | K3S_TOKEN=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX::server:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX sh -s - server --server https://10.XX.X.198:6443 --tls-san=network-loadbalancer-k3s- XXXXXXXXXXXXXXXX.elb.us-east-2.amazonaws.com  ----> On second and third master
 
curl -sfL https://get.k3s.io | K3S_TOKEN= XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX::server:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX sh -s - agent --server https://network-loadbalancer-k3s- XXXXXXXXXXXXXXXX.elb.us-east-2.amazonaws.com:6443    ----------------> To be run on all the Agents
```
![image](https://github.com/user-attachments/assets/6b9bebdb-5d19-4010-8b3d-5bdc71df22dc)

As shown in the architecture diagram for K3S HA cluster drawn above I had taken three masters (K3S Servers) and three nodes (K3S Agents). The etcd was treated as the cluster datastore. I had used a Network LoadBalancer in front of all the three K3S Servers as shown in the diagram drawn above to provide high availability. 

![image](https://github.com/user-attachments/assets/5b62894c-6282-401e-9625-98a276acf47d)

### K3S HA Cluster with PostgreSQL RDS as cluster datastore 
```
curl -sfL https://get.k3s.io | sh -s - server --cluster-init --datastore-endpoint="postgres://postgres:XXXXXXXX@dbinstance-1.XXXXXXXXXXXX.us-east-2.rds.amazonaws.com/mydb" --tls-san=network-loadbalancer-k3s-XXXXXXXXXXXXXXXX.elb.us-east-2.amazonaws.com:6443   -----> On First Master

cat /var/lib/rancher/k3s/server/node-token   --------------> Get the K3S Token from this command

curl -sfL https://get.k3s.io | K3S_TOKEN=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX::server:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX sh -s - server --server https://10.XX.X.198:6443 --datastore-endpoint="postgres://postgres:XXXXXXXX@dbinstance-1.XXXXXXXXXXXX.us-east-2.rds.amazonaws.com/mydb"  --tls-san=network-loadbalancer-k3s- XXXXXXXXXXXXXXXX.elb.us-east-2.amazonaws.com  ----> On second and third master
 
curl -sfL https://get.k3s.io | K3S_TOKEN= XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX::server:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX sh -s - agent --server https://network-loadbalancer-k3s- XXXXXXXXXXXXXXXX.elb.us-east-2.amazonaws.com:6443    ----------------> To be run on all the Agents
```
IP Address 10.XX.X.198 is the Private IP Address of first Master.

![image](https://github.com/user-attachments/assets/d8c5a2cb-acef-4ec3-90ef-21c929c013c7)

In the K3S HA (high availability) cluster configuration, each node registers with the Kubernetes API by using a fixed registration address (Network LoadBalancer). After registration, the agent nodes established a connection directly to one of the server nodes, as shown in the diagram above.

![image](https://github.com/user-attachments/assets/af90f3ce-31d3-4e98-ad18-1fdc00008a1c)
