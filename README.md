# Setting up a Standalone Spark Cluster with 1 Master Server in Digital Ocean

Important : If you implement this guide on your Cloud Provider, please be aware that there will be costs to pay, and it depends on the amount of compute resources and the time you have them online 

## Getting Started

This guide will describe the steps to create the cluster with Ansible

### Prerequisites

In order to follow along with this guide you must have the following 

* [DigitalOcean Account](https://www.digitalocean.com/) - The Cloud Provider
* [Add your ssh public key in Digital Ocean](https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/) - SSH
* [Terminal] - Access to the terminal or command line
* [ssh] - Access to the ssh command


### 1 Controller

#### 1.1 Create the controller machine

The controller machine is a droplet that contains all the tools needed to install the cluster

Please create a brand new droplet with these features

* [Centos 7.5 x64]
* [Droplet Spec: 2GB RAM/ 50 GB SSD/ $ 10 us per month ]
* [Data Center Region: Frankfurt]
* [Add your ssh key into such droplet]
* [Set the hostname :  controller]

#### 1.2  Access the controller machine

From your machine please execute this command to access the controller machine

```
ssh root@<new droplet ip>
```

Please replace <new droplet ip> with the ip from the recently created droplet

#### 1.3  Setup the controller

Once into the controller machine please execute this commands in order to install

* git
* ansible

```
yum update -y
yum install git vim -y
yum install epel-release -y
yum install ansible -y
```

#### 1.4  Generate an ssh key

Execute this command and leave the default options

```
[root@controller ~]# ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):<ENTER>
Enter passphrase (empty for no passphrase):<ENTER>
Enter same passphrase again:<ENTER>
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Xy1h/Sw30RcYZAZ+5RxT4D22I8vqeeZyyyh7LDC9zDk root@controller
The key's randomart image is:
+---[RSA 2048]----+
|           .o=+*o|
|          . o++.=|
|           .o.o*+|
|           ..o.o=|
|        S   o.o++|
|       o o ...ooo|
|        = =  o   |
|         E +=+   |
|         .B=*+.  |
+----[SHA256]-----+
```
This key will later be used to settup the cluster machines, we will configure a root access to the cluster machines

#### 1.5  Clone this repository

Execute this command

```
git clone https://github.com/abernalc/spark-ansible-recipe.git
```

### 2 Cluster Machines

#### 2.1 Create the amount of machines needed for the cluster 

Please create the amount of droplets needed for the Cluster with these features

* [Centos 7.5 x64]
* [Droplet Spec: 2GB RAM/ 50 GB SSD/ $ 10 us per month ]
* [Data Center Region: Frankfurt]
* [Add the controller ssh key into such droplets]
* [Add any hostname entry for this machines, it is irrelevant for this configuration]

### 3 Ansible Recipe

#### 3.1 In the Controller Machine : Modify the host file

Once the machines in DigitalOcean has been created, please grab all the ips from the machines

This configuration will setup only one Master Server 

Inside the controller machine get inside the folder created from step 1.5 

```
cd ~/spark-ansible-recipe
```

Edit the host file (with vim or nano, depends on which editor you manage best)

```
vim host
```

Populate the file

```
[spark_slaves]
<IP Machine 2>
<IP Machine 3>
<IP Machine 4>

[spark_masters]
<IP Machine 1>

[spark_servers:children]
spark_slaves
spark_masters
```

#### 3.2 Execute the ansible recipe

Execute the following command, within the root folder of the repository


```
ansible-playbook playbooks/spark/install-spark.playbook
```

### 4 Access the UI of the Cluster

#### 4.1 Enter with the next URL

Type the next url in the browser

We will use the ip of the machine 1, from the Cluster

```
http://<machine 1 ip>:8080
```
