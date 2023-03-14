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