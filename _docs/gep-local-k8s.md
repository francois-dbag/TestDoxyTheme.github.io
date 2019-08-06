---
layout: doc
title: "Local Kubernetes Deployment"
description: "Here is the description of the doc page"
date: 2018-11-08 8:14:30 +0600
post_image: assets/images/gep-high-level-components.jpg
category_name: Runtime - Kubernetes
category_slug: kubernetes
---
<b>Prerequisites:        
           1.Docker<br>
           2.Go<br>
           3.Kubernetes CLI(kubectl)<br>
           4.Helm and Tiller<br>
           5.KIND<br></b>


<b>Install Kubectl (Kubernetes CLI):</b>

To install kubectl on Linux

```$ curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl```

```$ chmod +x ./kubectl```

```$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH```

```$ echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc```

```$ kubectl version --short```

To install <b>“KIND”(kubernetes in Docker)</b> and Create a cluster,

 ```$ GO111MODULE="on" go get sigs.k8s.io/kind@v0.4.0 && kind create cluster```

After running above command hit the below to export the KUBECONFIG

```$ export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"```

To check the cluster info,

 ```$ kubectl cluster-info```


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

Once the above setup is completed now we have kubernetes environment in Local.<br>
Follow the below steps to deploy the genereated application.

<b>Create project and generate code from geppetto  application</b>

1. Clone the code in your local.

2. And enter into the local buildscript folder using the below commands to run the script which will build and push the docker images of microservices,load into kind and then deploy it in kind using helm charts.

```$ cd <APPNAME>/devops/local/buildscript```

To run the build script

```$ sh geppetto_build.sh```


Pod Description:








