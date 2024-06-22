### Need for  K8s

 1. _**High Availability (HA)**_ :
    + When we run our applications in  docker container and if the container fails, we need to manually start the container
    + If the node i.e. the machine fails all the containers running on the machine should be re-created on other machine
    + K8s can do both of the above
 2. _**Autoscaling**_
    + Containers don’t scale on their own.
    + Scaling is of two types
        + Vertical Scaling
        + Horizontal Scaling
    + K8s can do both horizontal and vertical scaling of containers
 3. _**Zero-Down time**_
    + K8s can handle  deployments with near zero-down time deployments
    + K8s can handle rollout (new version) and roll back (undo new version => previous version)
 4.  K8s is described as _**Production grade Container management**_

### History

* Google had a history of running everything on containers.
* To manage these containers, Google has developed container management tools (inhouse)
    + Borg
    + Omega
* With  Docker publicizing containers, With the experience in running and managing containers, Google has started a project,  _**Kubernetes (developed in Go)**_ and then handed it over to _**Cloud Native Container Foundation (CNCF)**_

#### Competetiors

* Apache Mesos
* Hashicorp Nomad
* Docker Swarm
* But K8s is clear winner

#### Terms

1. Distributed System
2. Node
3. Cluster
4. State
5. Stateful Applications
6. Stateless Applications
7. Monolith
8. Microservices
9. Desired State
10. Declarative vs Imperative
11. Pet Vs Cattle

###  K8s is not designed only for  Docker

* Initially k8s used  docker as a main container platform and docker used to get special treatment, from k8s 1.24 special treatment is stopped
* k8s is designed to run any container technology, for this  k8s expects container technology to follow k8s interfaces

### K8s Architecture

* Official Architecture image

![alt text](shots/2.PNG)

* Other easier representations
* Master Node

![alt text](shots/3.PNG)

* Node

![alt text](shots/4.PNG)

* Clients
    + kubectl
    + any rest based client

* Logical view

![alt text](shots/5.PNG)

### Kubernetes Components

* For the  k8s components article

    [ Refer Here : https://directdevops.blog/2019/10/10/kubernetes-master-and-node-components/ ]

* Control plane components (Master Node Components)
    + kube-api server
    + etcd (*)
    + kube-scheduler
    + controller manager
    + cloud controller manager
* Node Components
    + kubelet
    + kube-proxy
    + Container run time (*)

#### Kube-Api

* Handles all the communication of k8s cluster
* Let it be internal or external
* kube-api  server exposes functionality over HTTP(s) protocol and provides REST  API

#### Etcd

* For etcd

    [ Refer Here : https://etcd.io/ ]
* This is memory of k8s cluster

#### Scheduler

* Scheduler is responsible for creating  k8s objects and scheduling them on right node

#### Controller

* Controller Manger is responsible for maintaining desired state
* This reconcilation loop that checks for desired state and if it mis matches doing the necessary steps is done by controller

#### Kubelet

* This is an agent of the control plane

#### Container Runtime

* Container technology to be used in  k8s cluster
* in our case it is _**docker**_

#### Kube-Proxy

* This component is responsible for networking for containers on the node

#### kubectl

* This is command line that can be installed on the machine from which you communicate to k8s cluster 
* This tool is created to make communication with api-server simplified
* Kubectl has a config file (KUBECONFIG) which contains
    + api-server information
    + keys to communicate with  api  server
* Kubectl allows to communication with cluster to create resources
    * _**imperatively**_ : Type commands
    * _**declartively**_ : Write manifests (YAML files)
* Reads manifests and connects to  api server. Converts the manifest into REST API calls over JSON

### What is k8s manifest

* This is a yaml file which describes the desired state of what you want in/using  k8s cluster

### CI/CD Workflow

* Basic Workflow

![alt text](shots/6.PNG)

* Jenkins

![alt text](shots/7.PNG)

* Azure DevOps

![alt text](shots/8.PNG)

### IDEAL K8s HA-Cluster

![alt text](shots/9.PNG)

### Kubernetes as a Service

* All popular clouds are offering k8s as a service
    + AKS (Azure K8s service)
    + EKS (Elastic K8s Service)
    + GKE (Google K8s engine)
* All cloud providers manage control plane for you and they charge hourly. For nodes we pay the similar costs of virtual machines

### K8s Installations

* Single Node Installations
    + minikube
    + kind
* On-prem installations
    + kube-admin
* k8s as a Service
    + AKS
    + EKS
    + GKE
* Playground (for learning): 

    [ Refer Here : https://labs.play-with-k8s.com/ ]

#### Installing k8s cluster on ubuntu vms

* Create 3 ubuntu vms which are accesible to each other with atlest 2 vCPUS and 4 GB RAM
* Installation method (kubeadm) which is something we will be using in on-premises k8s.
* For kubeadm installation on single master node

    [ Refer Here : https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/ ]

#### Steps

* Install docker on all nodes
* Install CRI-Dockerd 

    [ Refer Here : https://github.com/Mirantis/cri-dockerd ]

* Run the below commands as root user in all the nodes
```
# Run these commands as root
###Install GO###
wget https://storage.googleapis.com/golang/getgo/installer_linux
chmod +x ./installer_linux
./installer_linux
source ~/.bash_profile

git clone https://github.com/Mirantis/cri-dockerd.git
cd cri-dockerd
mkdir bin
go build -o bin/cri-dockerd
mkdir -p /usr/local/bin
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
cp -a packaging/systemd/* /etc/systemd/system
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
systemctl daemon-reload
systemctl enable cri-docker.service
systemctl enable --now cri-docker.socket
```
* Installing kubadm, kubectl, kubelet 

    [ Refer Here : https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl ]

* Now create a cluster from a master node 

    [ Refer Here : https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/ ]

* Use the command `kubeadm init --pod-network-cidr "10.244.0.0/16" --cri-socket "unix:///var/run/cri-dockerd.sock"`
* Setup kubeconfig
* Install flannel `kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml`
* As a root user run kubeadm join commands (need to pass crisocket)
* Now from manager execute `kubectl get nodes`



### PODS
#### Imperative Way to manage k8s objects

* For official doc's

    [ Refer Here : https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/ ]



### Pod lifecycle

* K8s Pods will have following states :
 1. Pending
 2. Running
 3. Succeded
 4. Failed
 5. Unknown

### Container States in k8s pod

 1. Waiting
 2. Running
 3. Terminated

* For container states

    [ Refer here : https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-states ]

* IN pods we can specify container restart policy 

    [ Refer Here : https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy ]

### Container restart policy

* Let's try to create a short lived contianer with different restart policies
* for manifests with restartPolicy

    [ Refer here : https://github.com/asquarezone/KubernetesZone/commit/f136484d376b15c1a984a6d3058cc33e51615709 ]

    + always (Default)



    + never



    + exit code => success
    + exit code => failure



### Controllers in K8s

* Controllers are k8s objects which run other k8s resources. This k8s resource will be part of specification generally in template section.
* Controllers maintain desired state.
* Some of the controllers are :
    + Replication Controller/Replica Set
    + Stateful Sets
    + Deployments
    + Jobs
    + Cron Jobs
    + Daemonset

### K8s Jobs

* For official doc's

    [ Refer Here : https://kubernetes.io/docs/concepts/workloads/controllers/job/ ]

* K8s has two types of jobs
    + _**Job**_ : Run an activity/script to completion
    + _**CronJob**_ : Run an activity/script to completion at specific time period or intervals
* For the manifests with job and cronjob

    [ Refer here : https://github.com/asquarezone/KubernetesZone/commit/7531d6e0c2a712f287ee41e5d53d94c6af3643ac ]

* For jobs restartPolicy cannot be Always as job will never finish



* Jobs have backoffLimit to limit number of restarts and activeDeadline seconds to limit timeperiod of execution
* Running job and waiting for completion




* Cronjob manifest which we have written create a job every minute and waits for completion





#### Let's go back to Pods

* Let's run a alpine pod



* Now if we want to execute a command in the container of alpine pod



* To access the terminal



* _**Exercise**_ : If we have a pod with 2 container how exec a command on a specific container
* Let's run a pod which run application on some port 



* Now if we want to access the application in container we can do port-forward (not recommended approach)



### Controllers

* Controllers in  k8s control/maintain state of  k8s objects

![alt text](shots/10.PNG)

### ReplicaSet

* For ReplicaSet official doc's

    [ Refer Here : https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/ ]

* ReplicaSet is controller which maintains count of Pods as Desired State

#### RS - Activity1 : Create three nginx pods

* For the  nginx rs manifest without selector

    [ Refer Here : https://github.com/asquarezone/KubernetesZone/commit/77c8c9d804b4a69d9d4df9691dc696a740bf04ab ]

* Temporary workaround for adding selectors. For the changes added

    [ Refer here : https://github.com/asquarezone/KubernetesZone/commit/e52430db3bcf4e92990495741a5f4f1bc95db2ce ]

* Now apply the manifest



* Let's change the replica count



* We can increase (scale out) as well decrease (scale in) the replica count

![alt text](shots/11.PNG)

#### 
























