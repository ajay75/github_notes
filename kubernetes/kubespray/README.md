## Production level K8s with kubespray
- Kubespray is a set of ansible scripts and it allows you to provision, set up and configure kubernetes cluster environment in a extremely easy way.
## Hardware requirements
- 4 Nodes: Virtual/Physical Machines (one of four will be control node)
- Memory: 2GB
- CPU: 1 Core
- Hard disk: 20GB available

## Software requirements
- Ubuntu 16.04 OS / RedHat 8
- Python3
- git
- wget
- unzip
- terraform
- ansible ( will be installed via pip using kubespray repo)
- SSH Server 
- Privileged user
- Ansible version 2.4 or greater
- Jinja
## Networking requirements
- Internet access to download docker images and install softwares
- IPv4 Forwarding should be enabled
- Firewall should allow ssh access as well as ports required by Kubernetes. Internally open all the ports between node.

# On AWS or ON your local machine fulfill required softwares
- Create EC2 instance RHEL8 t2.small, tag name:jumpbox
- security group sg_jumpbox, in default VPC, only access from myip for security reason. region us-east-1
- ssh into the EC2 instance.
```
sudo yum -y update 
sudo yum install python3
sudo yum -y install git
sudo yum -y install wget
sudo yum -y install unzip
```
- check if ipv4 fowrward is enable: cat /proc/sys/net/ipv4/ip_forward if the output is 0 that means disable (it is by default disabled). To enable ipv4 forwarding run the command
```
sudo bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
check again 
cat /proc/sys/net/ipv4/ip_forward 
It should be 1
```
- Now disable the swap, to check swap use free -h, to turn off swapoff
```
free -h
swapoff -a
```
- Generate SSH between ansible controller and k8s nodes, leave the fields to default. This will generate public and private key. We will copy the public key to all the nodes.
```
ssh-keygen -t rsa

```
- git clone https://github.com/kubernetes-sigs/kubespray.git
- cd kubespray
- sudo pip3 install -r requirements.txt
- cd /tmp/
- wget https://releases.hashicorp.com/terraform/0.14.3/terraform_0.14.3_linux_amd64.zip
- unzip terraform_0.14.3_linux_amd64.zip 
- sudo mv /tmp/terraform /usr/local/bin/
- create an SSH key pair for Ansible on AWS EC2 (note the name of key created as it will be used in credentials.tfvars)
- cd contrib/terraform/aws/
- cp credentials.tfvars.example credentials.tfvars
- vi credentials.tfvars (just entered key name and region - programatic access key/secret access key for the IAM user)
- vi terraform.tfvars (aws_kube_master_num  = 2, aws_etcd_num  = 1, aws_kube_worker_num  = 3)
- terraform init
- terraform plan -out ajaysplan -var-file=credentials.tfvars
- terraform apply "ajaysplan"
- cd ~/kubespray
- cat inventory/hosts (noteL: the hosts file will be created once terraform plan execute successfully)
To copy file from local machine to aws in this case I generated key/pair file for jump instance and put in .ssh/kubespray/somefile.pem for ansible playbook to access. 
```
scp -i somekeyfile.pem somefile.txt ec2-user@<ip-address>.amazonaws.com:
sudo mv somekeyfile.pem /.ssh/kubespray/
eval $(ssh-agent)
ssh-add -D
ssh-add ~/.ssh/kubespray/somekeyfile.pem
```
- Note: If your variables.tf file is using ubuntu then ansible_user will be ubuntu, but if it is RHEL/CentOS/Amazon instance then change the user accordingly to your variables.tf file in "kubespray/contrib/terraform/aws/"
ansible-playbook -i ./inventory/hosts ./cluster.yml -e ansible_user=ubuntu -b --become-user=root --flush-cache
```
## Now connect to the bashtion host of the cluster locally and run kubectl command on bashtion host
```
ssh  -F ssh-bastion.conf core@<Private-ip-address-master-node>

kubectl get nodes
(if it returns errror Like: The connection to the server localhost:8080 was refused - did you specify the right host or port?

mkdir ~/.kube
sudo cp /etc/kubernetes/admin.conf ~/.kube/config
cd ~/.kube
sudo chown ubuntu:ubuntu config

kubectl get nodes 
(Should run successfully)

```
## Configure to run kubectl command from your local workstation
```
create a local folder .kube create a file using vi config, in this file copy and paste content of aws bashtion config file
cd ~/.kube
vi config
change the https:// <aws-load-balancer-dns>:6443

kubectl get nodes 
(should work locally)

```
