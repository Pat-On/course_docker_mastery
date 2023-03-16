# 133 Section Intro

- Storage
- Ingress
- CRD's
- Helm
- Dashboard
- Namespaces
- Future of K8s

---

# 134 Storage in Kubernetes

- storage and stateful workloads are harder in all systems
- containers make it both harder and easier than before
- `StatefulSets` is a new resource type, making Pods more sticky
- Bret's recommendation: avoid stateful workloads for first few deployments until you are good at the basics
  - use db-as-a-service whenever you can

## volumes in kubernetes

- creating and connecting Volumes: 2 types
- Volumes
  - ties to lifecycle of a Pod
  - all containers in a single pod can share them
- persistentVolumes
  - created at the cluster level, outlives a pod
  - separates storage config from pod using it
  - multiple pods can share them
- CSI plugins are the new way to connect to storage - Container Storage Interface - standard - provide plugins that can be made by cloud providers
  Links: - https://kubernetes.io/docs/concepts/storage/volumes/ - https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/ - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

---

# 135 Ingress

- None of our services types work at OSI Layer 7 (HTTP)
- How do we route outside connections based on hostname or URL
- Ingress Controller (optional) do this with 3rd party proxies
- Nginx is popular, but Treafik, HAProxy, F5, Envoy, Istio, Etc
  - Treafik was created with a idea of containers
- implementation is specific to Controller chosen

Links:

- https://kubernetes.io/docs/concepts/services-networking/ingress/
- https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/
- https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/
- https://doc.traefik.io/traefik/v2.0/providers/kubernetes-crd/#traefik-ingressroute-definition

---

# 136 CRD's and The Operator Pattern

- You can add 3rd party resources and controllers
- this extends kubernetes API and CLI
- A pattern is starting to emerge of using these together
- Operator: automate deployment and management of complex apps
  - eg databases monitoring tools, backups and custom ingress

Links:

- https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/
- https://kubernetes.io/docs/concepts/extend-kubernetes/operator/
- https://operatorhub.io/
- https://github.com/operator-framework

---

# 137 Higher Deployment Abstractions

- all our `kubectl` commands just talk to the Kubernetes APi
- Kubernetes has limited built-in templating, versioning, tracking, and management of your apps
- there are now over 60 3rd party tools to do that, but many are defunct
- `Helm` is the most popular
- `compose on Kubernetes` comes with Docker Desktop
- Remember these are optional, and your distro may have a preference
- most distros support Helm

## Templating YAML
- many of the deployment tools have templating options
- you will need a solution as the number of env/apps grow
- help was the first winner in this space, but can be comples
- official `Kustomize` feature works out-of-the-box (as of 1.14)
- `docker app` and compose-on-kubernetes are Docker's way (CNAB STANDARD)


Links:

- https://docs.google.com/spreadsheets/d/1FCgqz1Ci7_VCz_wdh8vBitZ3giBtac_H8SBw4uxnrsE/edit#gid=0
- https://github.com/docker/compose-on-kubernetes/tree/master/docs
- https://kubernetes.io/blog/2018/05/29/introducing-kustomize-template-free-configuration-customization-for-kubernetes/
- https://github.com/kubernetes-sigs/kustomize
- https://github.com/docker/app
- https://cnab.io/

---

# 138 Kubernetes Dashboard
## Web GUI for cluster Mgmt
- Default GUI for upstream Kubernetes
    - https://github.com/kubernetes/dashboard
- some distributions have their own GUI (Rancher, Docker Ent, OpenShift)
- lets you view resources and upload YAML
- Safety first!


# 139 Namespace/Context
- Namespaces limit scope, aka `virtual clusters`
- Not related to Docker/Linux namespaces
- won't need them in small clusters
- there are some built-in, to hide system stuff from `kubectl` users
    - `kubectl get namespaces`
    - `kubectl get all --all-namespaces`
- context change `kubectl` cluster and namespace
- see `~/.kube/config` file (it should be secret)
    - `kubectl config get-contexts`
    - `kubectl config set *`
    