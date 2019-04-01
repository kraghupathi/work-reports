# Setup MiniKF in Vagrantbox on Ubuntu_18.04

## System requirements

We recommend that your system meets the following requirements:

1. 12GB RAM
2. 2 CPUs
3. 50GB disk space

### Prerequisites

* Install [Vagrant](https://www.vagrantup.com/downloads.html)

* Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)

Install the vagrant plugins

> $ 

### Setup vagrant machine for MiniKF installation

Run the following commands to install MiniKF:

> $ vagrant box add 

> $ vagrant init kraghupathi/minikf

Add the following lines in Vagrantfile

> 


> $ vagrant up

This will take a few minutes to complete. Once the installation is completed successfully


Login to MiniKF VM

> $ vagrant ssh

### Deployment of BLE RSSI model

Install the following packages

> $ sudo apt-get update

> $ sudo apt-get install -y curl wget nfs-server python3-pip virtualenv

Installation of Kubeflow

> cd ~

> $ git clone https://github.com/ciscoAI/KFLab

> $ cd ~/KFLab/tf-mnist/minikf 

## Installation of Kubeflow

> $ cd ~/KFLab/tf-mnist

> $ ./install.bash

If there is any rate limit error from github, please follow the steps to generate token 

- Go to [Github Token Setup](https://github.com/settings/tokens)
- Copy the token and set an environment variable in your shell: _export GITHUB_TOKEN=<token>_.
 
## Setup train and serve stage on cluster

Run the training job setup script

> $ cd ~/KFLab/tf-mnist

> $ ./train.bash

> $ ./serve.bash

## Model Testing
### Using a local python client
Port forward to access the serving port locally

> $ cd ~/KFLab/tf-mnist

> $ ./portf.bash

Run a sample client code to predict images(See mnist-client.py)

> cd ~

> $ virtualenv --system-site-packages env

> $ source ~/env/bin/activate

> $ pip3 install --upgrade tensorflow

> $ pip3 install tensorflow-serving-api

> $ pip3 install python-mnist

> $ pip3 install Pillow

> $ pip3 install pandas

> $ TF_MNIST_IMAGE_PATH=data/7.png python mnist_client.py


### Kubeflow dashboard

How to log in

Use these credentials to log in to all services, for example, JupyterHub, Rok:

username: user

password: 12341234
