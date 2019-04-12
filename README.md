# Table of Contents
- [Automation script of Kubernetes cluster and Ksonnet using vagrant](#automation-script-of-kubernetes-cluster-and-ksonnet-using-vagrant)
    - [How to run Vagrantfile](#how-to-run-vagrantfile)

# Automation script of Kubernetes cluster and Ksonnet using vagrant

## How to run Vagrantfile

1. Clone the repo

    
       $ git clone https://github.com/CiscoAI/hcl-cisco-project1.git

       $ cd hcl-cisco-project1/vagrant/scripts

2. Change the corporate proxy and local ip address

   Change the corporate proxy (If you run behind the proxy) and local-ip/server-ip in **install.sh** file

3. Change the forward ports and private/public ip address

   Change the forward ports (To access the VM from outside the network ) and private/public ip's (if required) in **Vagrantfile**


   **Note:** Change/Update memory size, cpus..etc

4. Clone the box from vagrant cloud



       $ vagrant box add ubuntu/xenial64

5. Run the vagrantfile


       $ vagrant up

6. Loging into the vagrantbox


       $ vagrant ssh
