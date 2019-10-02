---
layout: doc
title: "Geppetto Local Setup"
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
    <li>Kind</li>
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

Follow the below steps for application deployment in aws.

<h5 class="heading-4">Clone the helm charts to run the geppetto aplication</h5>

1. Clone the repo,

```$ git clone <REPO URL>```

2. Get into the repo,

```$ gunzip -c <NAME>.tar.gz | tar xopf -```

It will untar the file.

3. Docker Login is required to access the Amazon ECR images(Get the login password from the team)

```docker login -u AWS -p <password> https://<aws_account_id>.dkr.ecr.<region>.amazonaws.com```

4. To install the helm charts,

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

Port-forwarding :

```$ kubectl get pods -n <NAMESPACE>```

```$ kubectl port-forward <CAMUNDA-POD-NAME> -n <NAMESPACE> 8081:8080```

You will find the dmn file in this path.

```geppettotest/application/services/Camunda/Gep_authorize.dmn``` 

```$ kubectl get svc -n <NAMESPACE>```

![getsvc gep](/assets/images/deployment/get-svc.png)

Endpoints: ```http://localhost:8081/engine-rest/deployment/create```

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

Port-forwarding :

```$ kubectl get pods -n <NAMESPACE>```

```$ kubectl port-forward <TELEMETRY-POD-NAME> -n <NAMESPACE> 8200:8200```

VaultUrl: ```http://localhost:8200/```

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

Need to delete the app-pod,generation-pod and system-entry pod deployments and upgrade the helm charts,

```kubectl get deloyments -n <NAMESPACE>```

```kubectl delete <DEPLOYMENT-NAME> -n <NAMESPACE>```

Delete the app-pod,generation-pod and system-entry pod

To upgrade the helm charts,

```helm upgrade geppetto-dist ./geppetto-dist```

To check the pods,

```kubectl get pods -n <NAMESPACE>```

To port-forward the UI and apigateway,

UI,
```kubectl port-forward <SYSTEM-ENTRY-POD-NAME> -n <NAMESPACE> 8082:80```

API-GATEWAY:

In another terminal,
```kubectl port-forward <SYSTEM-ENTRY-POD-NAME> -n <NAMESPACE> 31234:3000```


Geppetto is up in ```http://localhost:8082```

Now, you can generate applications from Geppetto local environment.

