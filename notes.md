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

