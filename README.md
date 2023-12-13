# kuberenetes-project

Agenda: Deploy the Application Using the Kubernetes Dashboard

Description: Your organization is looking to create a multi-tier application based on PHP and MySQL. Your job is to deploy this application using the Kubernetes dashboard. Create a user (service account) with the name of Sandry and make sure to assign her an admin role. WordPress and MySQL Pods should use node3 as an NFS storage server using static volumes. WordPress applications must verify the MySQL Service before getting it deployed. If the MySQL Service is not present, then the WordPress Pod should not be deployed. These all should be restricted to the namespace called cep-project1 and must have 3 svcs, and 3 Pods as a max quota. All sensitive data should be used using secrets and non-sensitive data using configmaps. 

Tools Required: kubeadm, kubectl, kubelet, and Docker  

Let's Start

Step 1: Deploy the dashboard
kubectl create -f https://raw.githubusercontent.com/sonal0409ORG/educka/master/dashboard/dashboard-insecure-v2.4.0.yml

Step 2: Create a namespace cep-project1
kubectl create ns cep-project1 

Step 3: Create a service Account with name as Sandry 
kubectl apply -f serviceaccount

Step 4: Create a cluster role binding to the service account Sandry and give access as cluster admin role 
kubectl create clusterrolebinding sandry-access --serviceaccount=cep-project1:sandry --clusterrole=cluster-admin

Prepare worker-node-1 to be NFS server and worker nodes to be the NFS client

NFS server on worker-Node-1:

Execute below steps on worker-node-1:

sudo mkdir /data

Install the NFS kernel server on the machine

sudo apt update
sudo apt install nfs-kernel-server


Change the owner user and group to nobody and nogroup
...

sudo chown nobody:nogroup /data/

Set permissions to 777 to allow everyone to read, write, and execute files in this directory

sudo chmod 777 /data/


Open the exports file in the /etc directory for permission to access the host server machine

...

sudo vi /etc/exports

Add the following code to the file:

/data *(rw,sync,no_root_squash)

Note: Exit the file and save the changes

...



Use the exportfs command to export all shared folders you registered in the /etc/exports file after making the appropriate changes

sudo exportfs -rv


Restart the NFS kernel server to apply the configuration changes

sudo systemctl restart nfs-kernel-server

Deploy NFS client on worker nodes
```
sudo apt update
sudo apt install nfs-common
```
===========

Deployment for mysql application
kubectl apply -f mysql

Deployment for wordpress application
kubectl apply -f wordpress

