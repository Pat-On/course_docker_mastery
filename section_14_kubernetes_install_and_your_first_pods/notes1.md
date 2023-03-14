# 104 Kubernetes Architecture Terminology

- Kubernetes: The whole orchestration system
  - K8s, "k-eight" or Kube for short
- Kubectl: CLI to configure Kubernetes and manage apps
  - using cube control official pronunciation
- node: single server in the kubernetes cluster
- Kubelet: kubernetes agent running on nodes
- control plane: set of containers that manage the cluster
  - Includes API server, scheduler, controller manager, etcd and moire
  - sometimes called the 'master'

Master

- etcd - is a consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.
- API -
- Scheduler -
- Controller Manager -
- Core DNS

Worker Nodes

- Kubelet (agent) -
- Kube-proxy - control networking

Read more about topic: https://kubernetes.io/docs/concepts/overview/components/#master-components

# 105 Kubernetes Local Install

- Kubernetes is a series of containers, CLI's and configurations
- Many ways to install, lets focus on easiest for learning
  - ram utilization
  - easily shutdown
  - easily reset?
  - easily used on local system
- Docker desktop: enable in settings
  - sets up everything inside docker's existing linux vm
- docker toolbox on Windows: minikube
  - uses virtualbox to make linux vm
- Your own linux host opr vm: microk8s
  - installs kubernetes right on the OS

## Kubernetes in a Browser

- play-with-k8s
- katacode

## docker desktop

- Runs/configures kubernetes master containers
- manages kubectl install and certs
- easily install, disable and remove from docker GUI

## Minikube

- download windows installer from giuthub
- minikube-installer.exe
- minikube start
- much like the docker-machine experience
- creates a virtualbox VM with kubernetes master setup
- does not install kubectl

## MicroK8s

- installs kubernetes (without docker engine) on localhost (linux)
- uses snap (rather than apt or yum) for install
- controll the microk8s service via microk8s commands
- kubectl accessable via microk8s kubectl
- add an alias to your shell (.bach_profile)
  - alias kubectl=microk8s.kubectl

Links:
https://killercoda.com/
https://kubernetes.io/docs/tasks/tools/#install-kubectl-on-windows
https://github.com/canonical/microk8s
https://github.com/kubernetes/minikube/releases/

# 106 Kubernetes Container Abstractions

- `Pod` - one or more containers running together on one Node
  - Basic unit of deployment. containers are always in pods
- `Controller`: for creating/updating pods and other objects
  - Many types of controllers inc. Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob, etc
- `Service`: network endpoint to connect to a pod
- `namespace`: filtered group of objects in cluster
- secrets, configMaps, and more

Links:

- https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/concepts/workloads/pods/
