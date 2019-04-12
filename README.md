# Table of Contents
- [Overview of the application](#overview-of-the-application)
    - [GKE Specific Structure](#gke-specific-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Setup](#setup)
- [Model Testing](#model-testing)
    - [Using a local Python client](#using-a-local-python-client)
    - [Using a web application](#using-a-web-application)
- [Extras](#extras)

# Overview of the application
This tutorial contains instructions to build an **end to end kubeflow app** on a
Kubernetes cluster running on Google Kubernetes Engine (GKE) with minimal prerequisites.

*It should work on any other K8s cluster as well.*
The blerssi model is trained and served from an NFS mount.

**This example is intended for
beginners with zero/minimal experience in kubeflow.**

This tutorial demonstrates:

* Train a simple BLERSSI Tensorflow model (*See blerssi_model.py*)
* Export the trained Tensorflow model and serve using tensorflow-model-server
* Test/Predict blerssi location with a python client(*See blerssi_client.py*)

# Prerequisites

1. **kubectl cli**

   Check if kubectl  is configured properly by accessing the cluster Info of your kubernetes cluster

        $ kubectl cluster-info

2. **ksonnet**

    Check ksonnet version

        $ ks version

    Ksonnet version must be greater than or equal to **0.11.0**. Upgrade to the latest if it is an older version

    Refer [link](https://github.com/ksonnet/ksonnet/releases)

If above commands succeeds, you are good to go !


# Installation

        $ git clone https://github.com/CiscoAI/hcl-cisco-project1

        $ cd hcl-cisco-project1/tf-blerssi

        $ ./install.bash


        # Ensure that all pods are running in the namespace set in variables.bash. The default namespace is kubeflow
        kubectl get pods -n kubeflow

If there is any rate limit error from github, please follow the instructions at:
[Github Token Setup](https://github.com/ksonnet/ksonnet/blob/master/docs/troubleshooting.md#github-rate-limiting-errors)


# Setup

1.  Build a custom image for training, create the training Image.

   Point `IMAGE` to your training image. 
   We are creating and storing the docker image in the local machine.
   To upload the image in the docker-hub and use it, refer the URL : https://github.com/CiscoAI/KFLab/tree/master/tf-mnist

       ```
       IMAGE=belrssi-demo:v1
       docker build . --no-cache  -f Dockerfile -t ${IMAGE}
       ```

2. Run the training job setup script

        ```
	      $ ./train.bash

        # Ensure that all pods are running in the namespace set in variables.bash. The default namespace is kubeflow
        kubectl get pods -n kubeflow
        ```

3. Start TF serving on the trained results

        ```
        $ ./serve.bash
        ```

# Model Testing

The model can be tested using a local python client or via web application

## Using a local python client

This is the easiest way to test your model if your kubernetes cluster does not
support external loadbalancers. It uses port forwarding to expose the serving
service for the local clients.

Port forward to access the serving port locally

       $ ./portf.bash


Run a sample client code to predict images(See blerssi-client.py)

      $ virtualenv -p python3 env
      $ pip3 install --upgrade tensorflow
      $ pip3 install tensorflow-serving-api
      $ pip3 install Pillow
      $ pip3 install python-mnist
      $ pip3 install pandas

      $ python3 blerssi-client.py

You should see the following result

     The output will be displayed with the predicted location in the CLI

## Using a web application

Point `IMAGE` to your training image. We are creating and storing the docker image in the local machine.   To upload the image in the docker-hub and use it, refer the [URL](https://github.com/CiscoAI/KFLab/tree/master/tf-mnist)

       ```
       IMAGE=belrssi-client:v1
       docker build . --no-cache  -f Dockerfile -t ${IMAGE}
       ```
Expose your web application on the Internet is NodePort. Define variables in variables.bash and run the following script:

      $ ./webapp.bash

After running this script, you will get the IP adress of your web application. Open browser and see app at http://IP_ADRESS:NodePort


# Extras

## Retrain your model

If you want to change the training image, set `image` to your new training
image. See the [prototype
generation](https://github.com/CiscoAI/kubeflow-workflows/blob/d6d002f674c2201ec449ebd1e1d28fb335a64d1e/mnist/train.bash#L21)

       $ ks param set ${JOB} image ${IMAGE}

If you would like to retrain the model(with a new image or not), you can delete
the current training job and create a new one. See the
[training](https://github.com/CiscoAI/kubeflow-workflows/blob/d6d002f674c2201ec449ebd1e1d28fb335a64d1e/mnist/train.bash#L28)
step.

       $ ks delete ${KF_ENV} -c ${JOB}
       $ ks apply ${KF_ENV} -c ${JOB}

## Clean up pods
	
	     $ ./cleanup.bash

   Forcefully terminate pods using:
   
   	   $ kubectl delete pod <pod_name> --force -n kubeflow --grace-period=0
	
### Note

If container needs to use an HTTP, HTTPS, or FTP proxy server (for internet connectivity), configure it by setting the environment variables when building docker image. Set the HTTP, HTTPS, or FTP proxy server environment variable in Dockerfile.
    
	     $ ENV HTTPS_PROXY "https://127.0.0.1:3001"
	     $ ENV HTTP_PROXY "http://127.0.0.1:3001"
	     $ ENV FTP_PROXY "ftp://127.0.0.1:3001"
	     $ ENV NO_PROXY "*.test.example.com,.example2.com"
    
