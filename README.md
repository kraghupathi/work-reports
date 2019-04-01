# Setup MiniKF in Vagrantbox on Ubuntu_18.04

## System requirements

We recommend that your system meets the following requirements:

1. 12GB RAM
2. 2 CPUs
3. 50GB disk space

### Prerequisites

* Install [Vagrant](https://www.vagrantup.com/downloads.html)

* Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)

#### Install the vagrant plugins

> $ vagrant plugin install vagrant-disksize

### Setup vagrant machine for MiniKF installation

Run the following commands to install MiniKF:

> cd ~

> $ git clone https://github.com/CiscoAI/hcl-cisco-project1

> cd ~/hcl-cisco-project1/vagrant/minikf

> $ vagrant box add kraghupathi/minikf

> $ vagrant up

This will take a few minutes to complete. Once the installation is completed successfully.


### Login to MiniKF VM

> $ vagrant ssh

### Deployment of mnist model
#### Setup corporate proxy (If run this setup behind the proxy)

> $ echo 'Acquire::https::Proxy "http://proxy.example.com:80/";' >> /etc/apt/apt.conf

> $ echo 'Acquire::http::Proxy "http://proxy.example.com:80/";' >> /etc/apt/apt.conf

> $ echo 'export http_proxy="http://proxy.example.com:80/"' >> /etc/profile

> $ echo 'export https_proxy="http://proxy.example.com:80/"' >> /etc/profile

> $ export no_proxy="10.10.10.10,127.0.0.1,localhost" 

#### Setup corporate proxy in docker engine (If run this setup behind the proxy)

> $ mkdir -p /etc/systemd/system/docker.service.d

> $ echo '[Service]' >> /etc/systemd/system/docker.service.d/http-proxy.conf

> $ echo 'Environment="HTTP_PROXY=http://proxy.example.com:80/"' >> /etc/systemd/system/docker.service.d/http-proxy.conf

> $ echo 'Environment="HTTPS_PROXY=http://proxy.example.com:80/"' >> /etc/systemd/system/docker.service.d/http-proxy.conf

> $ systemctl daemon-reload && service docker restart

### Install the following packages

> $ sudo apt-get update

> $ sudo apt-get install -y curl wget nfs-server python3-pip virtualenv

### Installation of Kubeflow and depoyment of Mnist

> cd ~

> $ git clone https://github.com/ciscoAI/KFLab

> $ cd ~/KFLab/tf-mnist/minikf

> $ ./install.bash

If there is any rate limit error from github, please follow the steps to generate token 

- Go to [Github Token Setup](https://github.com/settings/tokens)
- Copy the token and set an environment variable in your shell: _export GITHUB_TOKEN=<token>_.
 
## Run the training job setup script

> $ ./train.bash

## Start TF serving on the trained results

> $ ./serve.bash

Ensure that all pods are running in the namespace set in variables.bash. The default namespace is kubeflow

> $ sudo kubectl get pods -n kubeflow

## Model Testing
### Using a local python client

Port forward to access the serving port locally

> $ ./portf.bash

Run a sample client code to predict images(See mnist-client.py)

> cd ~

> $ virtualenv -p python3 env

> $ source ~/env/bin/activate

> $ pip3 install --upgrade tensorflow

> $ pip3 install tensorflow-serving-api

> $ pip3 install python-mnist

> $ pip3 install Pillow

> $ pip3 install pandas

> $ cd ~/KFLab/tf-mnist/minikf

> $ TF_MNIST_IMAGE_PATH=data/7.png python3 mnist_client.py


### Kubeflow dashboard

You can access the Kubeflow dashboard by opening a browser and going to:

http://10.10.10.10

#### How to log in

Use these credentials to log in to all services, for example, JupyterHub, Rok:

**username:** user

**password:** 12341234
