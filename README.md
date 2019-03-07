# Setting up a Standalone Spark Cluster in Digital Ocean

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
* [Data Center Region: Frankfurt]
* [Add your ssh key into such droplet]

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
yum install git -y
yum install epel-release -y
yum install ansible -y
```

#### 1.4  Clone this repository

Execute this command

```
git clone https://github.com/abernalc/spark-ansible-recipe.git
```
