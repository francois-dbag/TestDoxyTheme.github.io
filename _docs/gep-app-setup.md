---
layout: doc
title: "AWS Deployment-Geppetto"
description: "Here is the description of the doc page"
date: 2018-11-08 8:14:30 +0600
post_image: assets/images/gep-high-level-components.jpg
category_name: Runtime - Kubernetes
category_slug: kubernetes
---
<h5 class="heading-4">Create Your EC2 Resources and Launch Your EC2 Instance</h5>

Login to your instance and follow the steps below.

<h5 class="heading-4">Getting Started with eksctl</h5>

This getting started guide helps you to install all of the required resources to get started with Amazon EKS using eksctl, a simple command line utility for creating and managing Kubernetes clusters on Amazon EKS. At the end of this tutorial, you will have a running Amazon EKS cluster with worker nodes, and the kubectl command line utility will be configured to use your new cluster.

<h5 class="heading-4">Prerequisites</h5><br>
This section helps you to install and configure the binaries you need to create and manage an Amazon EKS cluster.

<h5 class="heading-4">Install the Latest AWS CLI</h5><br>
To use kubectl with your Amazon EKS clusters, you must install a binary that can create the required client security token for cluster API server communication. The aws eks get-token command, available in version 1.16.156 or greater of the AWS CLI, supports client security token creation.

If you already have pip and a supported version of Python, you can install or upgrade the AWS CLI with the following command:

```$ pip install awscli --upgrade --user```

```$ sudo pip install --upgrade pip```

<h5 class="heading-4">Configure Your AWS CLI Credentials</h5><br>
Both eksctl and the AWS CLI require that you have AWS credentials configured in your environment. The aws configure command is the fastest way to set up your AWS CLI installation for general use.

$ aws configure<br>
AWS Access Key ID [None]: YOUR_ACCESS_KEY<br>
AWS Secret Access Key [None]: YOUR_SECRET_KEY<br>
Default region name [None]: YOUR_REGION<br>
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

This procedure assumes that you have installed eksctl, and that your eksctl version is atleast 0.1.37. You can check your version with the following command:

```$ eksctl version```

1. Create your Amazon EKS cluster and worker nodes with the following command.

       $ eksctl create cluster \ 
        --name YOUR_CLUSTER_NAME \
        --version 1.13 \ 
        --nodegroup-name standard-workers \ 
        --node-type t2.large \
        --nodes 2 \ 
        --nodes-min 1 \
        --nodes-max 4 \
        --node-ami auto

2. Cluster provisioning usually takes between 10 and 15 minutes. 

<h5 class="heading-4">Install Kubectl (Kubernetes CLI):</h5>

To install kubectl on Linux

```$ curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl```

```$ chmod +x ./kubectl```

```$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH```

```$ echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc```

```$ kubectl version --client --short```

<b>sample output:</b><br>

        Client Version: v1.12.7

<h5 class="heading-4">To update kubeconfig file:</h5>     

```$ aws eks --region <YOUR-REGION> update-kubeconfig --name <CLUSTER-NAME>```

To check the cluster,

```$ kubectl cluster-info```

To Check the Worker-nodes,

  ```$ kubectl get svc```

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

<h5 class="heading-4">To deploy the AWS ALB Ingress controller into our Kubernetes cluster</h5>

Once the above setup is completed now we have kubernetes environment in AWS.<br>

Next, let’s deploy the AWS ALB Ingress controller into our Kubernetes cluster.<br>

Create the IAM policy to give the Ingress controller the right permissions:<br>

Go to the IAM Console and choose the section Policies.<br>
Select Create policy.<br>
Embed the contents of the template iam-policy.json in the JSON section.<br>

```https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.0.0/docs/examples/iam-policy.json```

Review policy and save as “ingressController-iam-policy”<br>
Attach the IAM policy to the EKS worker nodes:<br>

Go back to the IAM Console.<br>
Choose the section Roles and search for the NodeInstanceRole of your EKS worker node.<br> 
Example: eksctl-attractive-gopher-NodeInstanceRole-xxxxxx<br>
Attach policy “ingressController-iam-policy.”<br>

Deploy RBAC Roles and RoleBindings needed by the AWS ALB Ingress controller:<br>

```kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.0.0/docs/examples/rbac-role.yaml```

Sample Output:

clusterrole.rbac.authorization.k8s.io/alb-ingress-controller created
clusterrolebinding.rbac.authorization.k8s.io/alb-ingress-controller created
serviceaccount/alb-ingress created

Follow the below steps for application deployment in aws.

<h5 class="heading-4">Clone the helm charts to run the geppetto aplication</h5>

1. Clone the repo,

```$ git clone <REPO URL>```

2. Get into the repo,

```$ gunzip -c <NAME>.tar.gz | tar xopf -```

It will untar the file.

Before installing the helm charts you have to update the values.yaml with your certificate-arn and cluster name.

3. To install the helm charts,

```$ helm install --name geppetto-dist ./geppetto-dist```

To check the pods status,

```$ kubectl get namespace```

```$ kubectl get pods -n <NAMESPACE>```

![getpods gep](/assets/images/deployment/get-pods-gep.png)

<b>List of Pods Generated:</b>

1. app-pod<br>
2. app-db-pod<br>
3. system-entry<br>
4. camunda-pod<br>
5. generator-pod<br>
6. dev-ops-pod<br>
7. dev-ops-db<br>
8. telemetry-pod<br>

<b>1. App-pod:</b>

App-pod contains backend services for the generated applications which includes flowmanager, microflowmanager, projectmanager, screenmanager, entitymanager, featuremanager, securitymanager, camundamanager, proxymanager, adminmanager, menubuildermanager and templatemanager.

<b>2. App-db-pod:</b>
 
App-db-pod contains mongodb for the geppetto application.

<b>3. System-entry:</b>

System-entry pod contains the nginx container(ui) and api-gateway container.

<b>4. Camunda-pod:</b>

Camunda-pod used for authorization and authentication.

<b>5. Generator-pod:<br>

Generator-pod contains generator services for generationmanager, configurationmanager, redis, codegenmanager, backendgenmanager, datastoremanager, mongogenmanager, nodegenmanager, githubmanager, infrastructuremanager, frontendmanager, angulargenmanager, angulartemplatemanager,authgenmanager, admingenmanager, ionicmanager, buildmanager and deploymentmanager.

<b>6. Devops-pod:<br>

Dev-ops pods contains jenkins, nexus and sonarqube.

<b>7. Devops-db-pod:<br>

Devops-db-pod contains postgres db for the devops purpose.

<b>8. Telemetry-pod:</b>

Telemetry-pod contains the Elasticsearch, Fluentd and Kibana which will gives
the application logs.

<h5 class="heading-4">To load Camunda DMN file into the camunda stand-alone</h5>

To find the node port,

```kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[1].address}"```

It will return the ip of node.

You will find the dmn file in this path.

```geppettotest/application/services/Camunda/Gep_authorize.dmn``` 

```$ kubectl get svc -n <NAMESPACE>```

![getsvc gep](/assets/images/deployment/get-svc.png)

Endpoints: ```http://<YOUR-NODE-IP>:30060/engine-rest/deployment/create```

Header:<br>
"Content-Type":"application/json"<br>

Body:<br>
Form-data:<br>
data:DMN-file<br>
deployment-name:gep-authorize<br>
enable-duplicate-filtering:true<br>
deploy-changed-only:true<br>

![camubda post](/assets/images/deployment/camunda-post.png)

<h5 class="heading-4">To load mongo connection string and github details in Vault</h5>

To find the node port,

```kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[1].address}"```

It will return the ip of the worker-node.

VaultUrl: ```http://<YOUR-NODE-IP>:30082/```

Sign in to the vault using the token(Find the vault-token in values.yaml)

![vault](/assets/images/deployment/vault.png)

Enable the "kv" secret engine "version:1" 

![kv](/assets/images/deployment/enablesecrets.png)

![kv_version](/assets/images/deployment/enablesecretengine2.png)

To load the mongo connection string,<br>
PATH:"kubernetes/database/mongo/connection"<br>
KEY:"mongo_connection_string"<br>
VALUE:"mongodb://gep-stage-app-db.gep-stage-201908.svc.cluster.local:27017/GeppettoStage"<br>

To load the github credentials where you want to generate the application,<br>
PATH:"kubernetes/sourcecode/github"<br>
KEY:"email"<br>
VALUE:"Your github email"<br>
KEY:"username"<br>
VALUE:"Your github username"<br>
KEY:"password"<br>
VALUE:"Your github password"<br>

![mongo secret](/assets/images/deployment/mongosecret.png)

<h5 class="heading-4">Adding the seed files into the containers</h5>

```$ kubectl get pods -n <NAMESPACE>```

Use below command to get into the container,

```$ kubectl exec -ti <GENERATOR-POD-NAME> bash -n <NAMESPACE>```

```$ cd /geppetto/```

```$ mkdir generated-code```

```$ cd ..```

```$ mkdir template```

```$ cd ..```

Clone the tar file and extract to copy the seed file into the template folder

Get into extracted geppetto folder,

```$ cp -a seed /geppetto/template/```

It will copy the seed folder to template.

At last we need to redeploy the services once again for that,

<h5 class="heading-4">SSL setup</h5>

```$ kubectl get ingress -n <NAMESPACE>```

You will find the elb endpoint.You can check by hitting the url in browser.

In google domains add the HOST to the CNAME.

Now you can start to generate applications from Geppetto :)



