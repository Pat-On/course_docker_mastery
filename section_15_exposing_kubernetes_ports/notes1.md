# 115 Service Types

## Exposing Containers

- `kubectl expose` creates a service for existing pods
- a service is a stable address for pod(s)
- if we want to connect to pod(s) we need a service
- CoreDNS allows us to resolve service by name - one of the core feature of control plan
- there are different types of services

- these service are always available in Kubernetes:

  - clusterIP
    - default
    - single, internal virtual IP allocated
    - only reachable from within cluster (node and pods)
    - pods can reach serviced on apps port number
    - cluster ip is good only in cluster
  - NodePort

    - high port allocated on each node
    - port is open on every node's ip
    - anyone can connect (if they can reach node)
    - other pods need to be updated to this port

  - LoadBalancer
    - mostly used in the cloud
    - controls a LB endpoint external to the cluster
    - only available when infra provider give you a lb (AWS ELB)
    - creates nodePort + clusterIp services, tells LB to send to NodePort
    - only for traffic coming to your kubernetes
  - ExternalName

    - nothing to do with inbound traffic
    - adds CNAME DNS records to CoreDNS only
    - not used for pods but for giving pods a DNS name to use for something outside Kubernetes

  - Kubernetes Ingress: we will learn later

---

# 118 Creating Netshoot in Kubernetes

Links:
https://coredns.io/
https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/

## Creating a ClusterIP service

- Open two shell windows so we can watch this
  - `kubectl get pods -w`
- in second window, lets start a simple http server using sample code
  - `kubectl create deployment httpenv --image=bretfisher/httpenv`
- scale it to 5 replicas
  - `kubectl scale deployment httpenv --replicas=5`
- Let's create a CluserIP service (default)
  - `kubectl expose deployment/httpenv --port 8888`

## Checking created services

` kubectl get service`

```
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
httpenv      ClusterIP   10.100.88.68   <none>        8888/TCP   66s
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP    23h
```

- Remember this ip is cluster internal only, how do we curl it?
- if you are on docker desktop (host os is not container os)
  - OLD command
  - `kubectl run --generator=run-pod/v1 tmp-shell --rm -it --image bretfisher/netshoot -- bash`
  - NEW command
  - `kubectl run tmp-shell --rm -it --image bretfisher/netshoot -- bash`
- we need to run another cluster lol
- single pod as a entry port

All you're doing differently is removing the --generator option from the video slide, which is no longer needed with kubectl run.

`bash-5.1# curl 10.100.88.68:8888`

```
{"HOME":"/root",
"HOSTNAME":"httpenv-844f545f9-tznmt",
"KUBERNETES_PORT":"tcp://10.96.0.1:443",
"KUBERNETES_PORT_443_TCP":"tcp://10.96.0.1:443",
"KUBERNETES_PORT_443_TCP_ADDR":"10.96.0.1",
"KUBERNETES_PORT_443_TCP_PORT":"443",
"KUBERNETES_PORT_443_TCP_PROTO":"tcp",
"KUBERNETES_SERVICE_HOST":"10.96.0.1",
"KUBERNETES_SERVICE_PORT":"443",
"KUBERNETES_SERVICE_PORT_HTTPS":"443",
"PATH":"/usr/local/sbin:/usr/local/bin:/usr/
sbin:/usr/bin:/sbin:/bin"}bash-5.1#
```

- `curl httpenv:8888`
- `curl [ip of service]:8888`
