# Steps-to-create-Kubernetes-cluster

prerequisites to create Kubernetes cluster

1) Need a Sub Domain name.(kops.domain.com)

2) Create a linux VM in EC2

--> create t2.micro machine with Ubuntu to create kubernetes cluster

3) Install Kops, Kubectl, ssh Keys, awscli

```
sudo apt-get update
````
```
sudo apt-get upgrade
````
```
sudo ssh-keygen
````
```
sudo apt install awscli -y
````
```
aws configure
````
(here configure access key, secretkey, origin and language as per the IAM user configuration)
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````
```
sudo chmod +x ./kubectl
````
```
sudo mv kubectl /usr/local/bin/
````
```
kubectl --help
````
```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest
````
```
sudo chmod +x ./kops-linux-amd64
````
```
sudo mv kops-linux-amd64 /usr/local/bin/kops
````

***4) Login AWS and setup s3 bucket, IAM user for AWScli, Route 53 Hosted Zone.***

login to aws console create s3 bucket

Create IAM user and give adminfull access permissions. next configure accesskey and secretkey in machine using 'aws configure' cmd

Create hosted zone in Route 53 and configure name space in godaddy domain.

Configure ns names with sub domain name i godaddy, as per the Hosted zones configued in aws route 53 


***cmd to create kubernetes cluster***

```
kops create cluster --name=kubevpro.groophy.in \                                              
````
create cluster with name as per our requirement
```
--state=s3://vprofile-kop-states --zones=us-east-2a,us-east-2b \                              
````
give s3 bucket access to the cluster
```
--node-count=2 --node-size=t3.small --master-size=t3.medium --dns-zone=kubevpro.groophy.in \ 
````
create worker nodes and master node and dnz zone as our requirement

```
--node-volume-size=8 --master-volume-size=8                                                  
````
create required volumes size to allocate to worker nodes and master node      

cmd to create the cluster as per the above configuration

```
kops update cluster --name=kops.sanelahealth.com --state=s3://kopsbucket-testproject --yes --admin
````

cmd to validate the cluster status

```
kops validate cluster --name=kops.sanelahealth.com --state=s3://kopsbucket-testproject
````
check the available node

```
kubectl get node
````


cmd to remove kubernetes cluster
```
kops delete cluster --name=kops.sanelahealth.com --state=s3://kopsbucket-testproject --yes
````
