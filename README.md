# HELM REDIS

<a href="https://ssh.cloud.google.com/cloudshell/editor?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Famit-shochat%2Fhelm-redis">
<img alt="Open in Cloud Shell" src="https://gstatic.com/cloudssh/images/open-btn.svg"></a>

***if you have a kubernetes access on google cloud PLZ check the link up***


**For change the admin pass!**

**The AUTH password is ADMIN**

in the templates/configMap.yaml in line #514 change the word admin to your new password
<pre>
...
   # 150k passwords per second against a good box. This means that you should
    # use a very strong password otherwise it will be very easy to break.
    #
    requirepass admin

    # Command renaming.
...
</pre>


**Guide for Deploy redis with passord to AUTH and DEBUG mode**

please clone the repo to your DESKTOP and chenge dir to redis
>it clone https://github.com/amit-shochat/helm-redis && cd "$(basename "$_" .git)"

***chart tree*** 

<pre>
.
├── Chart.yaml
├── README.md
└── templates
    ├── configMap.yaml
    ├── _helpers.tpl
    └── redis-databass.yaml
</pre>

Please be sure you have kubectl and helm installed and you have kubernetes access
you can use minikube app

for install HELM you can check here: 
https://helm.sh/docs/intro/install/

for install minikube you can check here:
https://minikube.sigs.k8s.io/docs/start/

***start Minikube application*** 
>minikube start 
<pre>
$ minikube start  
😄  minikube v1.23.2 on Debian 
    ▪ MINIKUBE_HOME=/home/blablalba/bla/blabla/.minikube
✨  Automatically selected the docker driver
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=3800MB) ...
🐳  Preparing Kubernetes v1.22.2 on Docker 20.10.8 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
</pre>

***if you choose to use minikube you need to open tunnel***
>└─$ minikube tunnel -c

***delpoy redis to Kubernetes***
>helm install redis .  
<pre>
$ helm install redis .  
NAME: redis
LAST DEPLOYED: Sun Oct 31 19:47:52 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
</pre>

***GET PODS INFO***
> kubectl get pods
<pre>
$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
redis-deployment-5b6947cdb7-w6g2k   1/1     Running   0          87s
</pre>

***GET SERVICE AND IP TO CONNECT THE REDIS***
>$ kubectl get service  
<pre>
$ kubectl get service
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE
kubernetes      ClusterIP      10.96.0.1        <none>           443/TCP          11m
redis-service   LoadBalancer   10.110.214.216   10.110.214.216   6379:30942/TCP   5m35s
</pre>

**connect the redis**
##plz downloud redis-cli and install it##


if you get Running on pods status and EXTERNAL-IP now you can access to the server 
> redis-cli -h YOUR_EXTERNAL-IP -p 6379 

and let try to enter valuse
>SET linux rule

but we got a erro message 
<pre>
10.110.214.216:6379> SET linux ruls
(error) NOAUTH Authentication required.
</pre>

we need to pass the AUTH  
<pre>
10.110.214.216:6379> AUTH admin
OK
</pre>
and try agine 
<pre>
10.110.214.216:6379> SET linux ruls
OK
10.110.214.216:6379> GET linux
"ruls"
</pre>

