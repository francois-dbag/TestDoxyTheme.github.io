---
layout: doc
title: "AWS Kubernetes Deployment"
description: "Here is the description of the doc page"
date: 2018-11-08 8:14:30 +0600
post_image: assets/images/gep-high-level-components.jpg
category_name: Runtime - Kubernetes
category_slug: kubernetes
---

<h5 class="heading-4">Getting Started with eksctl</h5>

This getting started guide helps you to install all of the required resources to get started with Amazon EKS using eksctl, a simple command line utility for creating and managing Kubernetes clusters on Amazon EKS. At the end of this tutorial, you will have a running Amazon EKS cluster with worker nodes, and the kubectl command line utility will be configured to use your new cluster.

<h5 class="heading-4">Prerequisites</h5><br>
This section helps you to install and configure the binaries you need to create and manage an Amazon EKS cluster.

<h5 class="heading-4">Install the Latest AWS CLI</h5><br>
To use kubectl with your Amazon EKS clusters, you must install a binary that can create the required client security token for cluster API server communication. The aws eks get-token command, available in version 1.16.156 or greater of the AWS CLI, supports client security token creation.

If you already have pip and a supported version of Python, you can install or upgrade the AWS CLI with the following command:

``` $ pip install awscli --upgrade --user```

<h5 class="heading-4">Configure Your AWS CLI Credentials</h5><br>
Both eksctl and the AWS CLI require that you have AWS credentials configured in your environment. The aws configure command is the fastest way to set up your AWS CLI installation for general use.

$ aws configure<br>
AWS Access Key ID [None]: YOUR_ACCESS_KEY<br>
AWS Secret Access Key [None]: YOUR_SECRET_KEY<br>
Default region name [None]: us-east-1<br>
Default output format [None]: json<br>

<h5 class="heading-4">Install eksctl</h5>

<b>To install or upgrade eksctl on Linux using curl</b>

 1. Download and extract the latest release of eksctl with the following command.

``` $ curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp```

 2. Move the extracted binary to /usr/local/bin.

```$ sudo mv /tmp/eksctl /usr/local/bin```

 3. Test that your installation was successful with the following command

```$ eksctl version```

Create Your Amazon EKS Cluster and Worker Nodes
Now you can create your Amazon EKS cluster and a worker node group with the eksctl command line utility.

<h5 class="heading-4">To create your cluster and worker nodes with eksctl</h5>

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

<h5 class="heading-4">Install Kubectl (Kubernetes CLI):</h5>

To install kubectl on Linux

```$ curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl```

```$ chmod +x ./kubectl```

```$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH```

```$ echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc```

```$ kubectl version --short```

<b>sample output:</b><br>

        Client Version: v1.12.7<br>
        Server Version: v1.12.10-eks-2e569f<br>

<h5 class="heading-4">Install Helm and Tiller :</h5>

<b>Helm install :</b><br>

```$ curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh``` <br>
```$ chmod 700 get_helm.sh```<br>
```$ ./get_helm.sh```<br>

<b>Tiller install :</b><br>

```$ kubectl -n kube-system create sa tiller```<br>
```$ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller```<br>
```$ helm init --service-account tiller```<br>


<b>Check installation :</b>

```$ helm version --short```

<b>Sample Output :</b>

  Client: v2.14.1+g5270352<br>
  Server: v2.14.1+g5270352<br>

Once the above setup is completed now we have kubernetes environment in AWS.<br>
Follow the below steps for application deployment in aws.


<h5 class="heading-4">Create project and generate code from geppetto application</h5>

1. Clone the code.

2. And enter into the cloud buildscript folder using the below commands to run the script which will build and push the docker images of microservices,push into dockerhub and then deploy it using helm charts.

```$ cd <APPNAME>/devops/cloud/buildscript```

To run the build script

```$ sh geppetto_build.sh```

![Aws Helm](/assets/images/deployment/aws-helm.png)

After the successful run,You will find the Application url and logging Url.

To check the pods status

```$ kubectl get pods -n <NAMESPACE>```

![Aws Pods](/assets/images/deployment/aws-getpods.png)

<b>List of Pods Generated:</b>

1. app-pod<br>
2. app-db-pod<br>
3. system-entry<br>
4. camunda-pod<br>
5. telemetry-pod<br>

<b>1. App-pod:</b>

App-pod contains backend services for the generated applications which includes adminmanager,authproxy,camunda and security manager.

<b>2. App-db-pod:</b>
 
App-db-pod contains mongodb for the generated applications.

<b>3. System-entry:</b>

System-entry pod contains the nginx container(ui) and api-gateway container.

![ui aws](/assets/images/deployment/login-aws.png)

<b>4. Camunda-pod:</b>

Camunda-pod used for authorization and authentication.

<b>5. Telemetry-pod:</b>

Telemetry-pod contains the Elasticsearch, Fluentd and Kibana which will gives
the application logs.

Create index pattern in management section to retrieve data from Elasticsearch.

![landing kibana](/assets/images/deployment/logstash.png)

Choose the configure setting in Step:2,

![step2 kibana](/assets/images/deployment/step2_kibana.png)

You can see the container logs in Discover section,

![kibana logs](/assets/images/deployment/kibanalogs_initial.png)

![conatiner logs](/assets/images/deployment/kibana-logs.png)

![Geppetto High Level](/assets/images/gep-high-level-components.jpg)
