---
layout: doc
title: "AWS Kubernetes Deployment"
description: "Here is the description of the doc page"
date: 2018-11-08 8:14:30 +0600
post_image: assets/images/gep-high-level-components.jpg
category_name: Runtime - Kubernetes
category_slug: kubernetes
---

<b>Getting Started with eksctl</b><br>

This getting started guide helps you to install all of the required resources to get started with Amazon EKS using eksctl, a simple command line utility for creating and managing Kubernetes clusters on Amazon EKS. At the end of this tutorial, you will have a running Amazon EKS cluster with worker nodes, and the kubectl command line utility will be configured to use your new cluster.

<b>Prerequisites</b><br>
This section helps you to install and configure the binaries you need to create and manage an Amazon EKS cluster.

<b>Install the Latest AWS CLI</b><br>
To use kubectl with your Amazon EKS clusters, you must install a binary that can create the required client security token for cluster API server communication. The aws eks get-token command, available in version 1.16.156 or greater of the AWS CLI, supports client security token creation.

If you already have pip and a supported version of Python, you can install or upgrade the AWS CLI with the following command:

``` $ pip install awscli --upgrade --user```

<b>Configure Your AWS CLI Credentials</b><br>
Both eksctl and the AWS CLI require that you have AWS credentials configured in your environment. The aws configure command is the fastest way to set up your AWS CLI installation for general use.

$ aws configure<br>
AWS Access Key ID [None]: YOUR_ACCESS_KEY<br>
AWS Secret Access Key [None]: YOUR_SECRET_KEY<br>
Default region name [None]: us-east-1<br>
Default output format [None]: json<br>

<b>Install eksctl</b>

To install or upgrade eksctl on Linux using curl

1. Download and extract the latest release of eksctl with the following command.

``` $ curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp```

2. Move the extracted binary to /usr/local/bin.

```$ sudo mv /tmp/eksctl /usr/local/bin```

3. Test that your installation was successful with the following command

```$ eksctl version```

Create Your Amazon EKS Cluster and Worker Nodes
Now you can create your Amazon EKS cluster and a worker node group with the eksctl command line utility.

<b>To create your cluster and worker nodes with eksctl</b>

This procedure assumes that you have installed eksctl, and that your eksctl version is at least 0.1.37. You can check your version with the following command:

```$ eksctl version```

1. Create your Amazon EKS cluster and worker nodes with the following command.

       $ eksctl create cluster \ 
        --name YOUR_CLUSTER_NAME \
        --version 1.13 \ 
        --nodegroup-name standard-workers \ 
        --node-type t3.medium \
        --nodes 2 \ 
        --nodes-min 1 \
        --nodes-max 4 \
        --node-ami auto

2. Cluster provisioning usually takes between 10 and 15 minutes. When your cluster is ready, test that your kubectl configuration is correct.

  ```$ kubectl get svc```

<b>Install Kubectl (Kubernetes CLI):</b>

To install kubectl on Linux

```$ curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl```

```$ chmod +x ./kubectl```

```$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH```

```$ echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc```

```$ kubectl version --short```

<b>sample output:</b><br>

        Client Version: v1.12.7<br>
        Server Version: v1.12.10-eks-2e569f<br>

<b>Install Helm and Tiller :</b>

Helm install :<br>

```$ curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh``` <br>
```$ chmod 700 get_helm.sh```<br>
```$ ./get_helm.sh```<br>

Tiller install :<br>

```$ kubectl -n kube-system create sa tiller```<br>
```$ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller```<br>
```$ helm init --service-account tiller```<br>


Check installation:

```$ helm version --short```

Sample Output : 

  Client: v2.14.1+g5270352<br>
  Server: v2.14.1+g5270352<br>

Once the above setup is completed now we have kubernetes environment in AWS.<br>
Follow the below steps for application deployment in aws.


<b>Create a project from Geppetto Application:</b>

List of Pods Generated 

1. app-pod<br>
2. app-db-pod<br>
3. system-entry<br>
4. camunda-pod<br>

![Geppetto High Level](/assets/images/gep-high-level-components.jpg)
