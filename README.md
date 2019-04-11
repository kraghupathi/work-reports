# Table of Contents
- [Setup MiniKF in Vagrantbox on Ubuntu_18.04](#setup-minikf-in-vagrantbox-on-ubuntu_18.04)
    - [System requirements](#system-requirements)
    - [Prerequisites](#prerequisites)
    - [Setup vagrant machine](#setup-vagrant-machine)
    - [Installation and Setup Kubeflow Lab](#installation-and-setup-kubeflow-ab)
    - [Kubeflow dashboard](#kubeflow-dashboard)

# Setup MiniKF in Vagrantbox on Ubuntu_18.04
## System requirements
We recommend that your system meets the following requirements:

* 12GB RAM
* 2 CPUs
* 50GB disk space

## Prerequisites


1. **Install Vagrant**

   Refer [link](https://www.vagrantup.com/downloads.html) to install vagrant

2. **Install Virtualbox**

    Refer [link](https://www.virtualbox.org/wiki/Downloads) to install virtualbox

3. **Install vagrant plugins**

        
        $ vagrant plugin install vagrant-disksize


## Setup vagrant machine

1.  Clone the repo to setup the minikf

        
        $ git clone https://github.com/CiscoAI/hcl-cisco-project1

      
        $ cd hcl-cisco-project1/vagrant/minikf

        
        $ vagrant box add kraghupathi/minikf

        
        $ vagrant up

This will take a few minutes to complete. Once the installation is completed successfully.

2.  Login to minikf VM

        
        $ vagrant ssh

## Installation and Setup Kubeflow Lab

Setup the corporate proxy (If run this setup behind the proxy)

1.  Add the following lines in /etc/apt/apt.conf file

       ```
       Acquire::http::Proxy "http://proxy.example.com:80/";
       Acquire::https::Proxy "http://proxy.example.com:80/";

       ```

2. Export the proxy

       ```
      export http_proxy="http://proxy.example.com:80/"
      export https_proxy="http://proxy.example.com:80/"
      export no_proxy="10.10.10.10,127.0.0.1,localhost"

       ```

3. Add the proxy in Docker enginer


        
        $ sudo mkdir -p /etc/systemd/system/docker.service.d

Add the following lines in /etc/systemd/system/docker.service.d/http-proxy.conf

       ```
       [Service]
       Environment="HTTP_PROXY=http://proxy.example.com:80/"'
       Environment="HTTPS_PROXY=http://proxy.example.com:80/"

       ```

Restart docker service

        
        $ sudo systemctl daemon-reload && sudo systemctl enable docker && sudo service docker restart

4. Install dependencies 
     
        
        $ sudo apt-get update && sudo apt-get install -y curl wget nfs-server python3-pip virtualenv

5. Setup Kubeflow and deploye the mnist model


Refer [link](https://github.com/CiscoAI/KFLab/blob/master/tf-mnist/README.md) to setup the kubeflow


## Kubeflow dashboard

You can access the Kubeflow dashboard by opening a browser and going to:

Open browser and see app at http://10.10.10.10

Use below credentials to log in to all services, for example, JupyterHub, Rok:

 ```
   username: user
   password: 12341234
 ```
    
