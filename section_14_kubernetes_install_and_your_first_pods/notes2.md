# 107 Kubectl, create and apply

- Kubernetes is evolving and so is the CLI
- We get three ways to create pods from the kubectl CLI
  - `kubectl run` (single pod per command since 1.18)
  - `kubectl create` (create some resources via CLI or YAML)
  - `kubectl apply` (create/update anything via YAML)
- For now we will just use `run` or `create` CLI
- Later we will learn YAML and pros/cons of each

# 108 Your first pod with kubectl run

Creating a Pod with kubectl

- are we working

  - kubectl version
    - `kubectl version --short` (new version)

- two ways do deploys pods (containers): via commands or via yamls
- lets run a pod of the nginx web server!
  - `kubectl run my-nginx --image nginx`
- Let's list the pod
  - `kubectl get pods`
- lets see all object
  - `kubectl get all`
  - there is 80 or 70 resources out of the box that we are not dealing
  - so it does not give us everything

## Pods: Why do they exist?
-  Unlike Docker, you can not create a container directly in K8s
- You create Pod(s) (via CLI, YAML, or API)
    - Kubernetes then creates the container(s) inside it
- kubelet tells the container runtime to create containers for you
- every type of resource to run containers uses pods

- Sum: 
    - kubernetes only create pods and kubelet agent is creating the containers using docker runtime! 


links:
https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/
https://kubernetes.io/docs/reference/kubectl/cheatsheet/
https://cheatography.com/deleted-44122/cheat-sheets/kubectl/
https://cheatography.com/gauravpandey44/cheat-sheets/kubernetes-k8s/