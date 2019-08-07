---
layout: doc
title: "Local Kubernetes Deployment"
description: "Here is the description of the doc page"
date: 2018-11-08 8:14:30 +0600
post_image: assets/images/gep-high-level-components.jpg
category_name: Runtime - Kubernetes
category_slug: kubernetes
---

<h5 class="heading-4">Prerequisites: 
</h5>  

<ul class="unorder-list">
    <li>Docker</li>
    <li>Go</li>
</ul>

<h5 class="heading-4">Install Kubectl (Kubernetes CLI): 
</h5>

```$ curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl```

```$ chmod +x ./kubectl```

```$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH```

```$ echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc```

```$ kubectl version --short```

<h5 class="heading-4">To install “KIND”(kubernetes in Docker) and Create a cluster,</h5>

```$ GO111MODULE="on" go get sigs.k8s.io/kind@v0.4.0 && kind create cluster```

After running above command hit the below to export the KUBECONFIG

```$ export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"```

To check the cluster info,

```$ kubectl cluster-info```


<h5 class="heading-4">Install Helm and Tiller :</h5>

<b>Helm install :</b><br>

```$ curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh``` <br>
```$ chmod 700 get_helm.sh```<br>
```$ ./get_helm.sh```<br>

<b>Tiller install :</b><br>

```$ kubectl -n kube-system create sa tiller```<br>
```$ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller```<br>
```$ helm init --service-account tiller```<br>


<b>Check installation:</b><br>

```$ helm version --short```

<b>Sample Output :</b><br>

  Client: v2.14.1+g5270352<br>
  Server: v2.14.1+g5270352<br>

Once the above setup is completed now we have kubernetes environment in Local.<br>
Follow the below steps to deploy the genereated application.

<h5 class="heading-4">Create project and generate code from geppetto application</h5>

1. Clone the code in your local.

2. And enter into the local buildscript folder using the below commands to run the script which will build and push the docker images of microservices,load into kind and then deploy it in kind using helm charts.

```$ cd <APPNAME>/devops/local/buildscript```

To run the build script

```$ sh geppetto_build.sh```

![Local Helm](/assets/images/deployment/local-helm.png)

Port-forwarding :

```$ kubectl get pods -n <NAMESPACE>```

```$ kubectl port-forward <SYSTEM-ENTRY-POD-NAME> -n <NAMESPACE> 31234:4000```

![portforward-apigateway](/assets/images/deployment/apigateway-portforward.png)

Open another terminal tab,

```$ kubectl port-forward <SYSTEM-ENTRY-POD-NAME> -n <NAMESPACE> 4222:80```

![portforward-desktop](/assets/images/deployment/desktop-portforward.png)

You can access Application in your browser at http://localhost:4222/,

![local login](/assets/images/deployment/local-login.png)


<h5 class="heading-4">List of Pods Generated:</h5>

1. app-pod<br>
2. app-db-pod<br>
3. system-entry<br>
4. camunda-pod<br>

<b>1. App-pod:</b>

    App-pod contains backend services for the generated applications which includes adminmanager,authproxy,camunda and security manager.

<b>2. App-db-pod:</b>
 
    App-db-pod contains mongodb for the generated application.

<b>3. System-entry:</b>

    System-entry pod contains the nginx container(ui) and api-gateway container.

<b>4. Camunda-pod:</b>

    Camunda-pod used for authorization and authentication.

<h5 class="heading-4">Logging</h5>

To see the logs of the container,

```$ kubectl logs <POD-NAME> <CONTAINER-NAME> -n <NAMESPACE>```

![container logs](/assets/images/deployment/container-logs.png)

    

    

     







    
      
   
    








