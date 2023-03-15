# 119 Creating a NodePort and LoadBalancer Service

read more:
https://kubernetes.io/docs/concepts/services-networking/service/#nodeport

## Create a Node Port Service

- Let's expose a NodePort so we can access it via the host IP
  (including localhost on Windows/Linux/MacOS)
  - `kubectl expose deployment httpenv --port 8888 --name httpenv-np --type NodePort`

`$ kubectl get all`

```
NAME                          READY   STATUS    RESTARTS   AGE
pod/httpenv-844f545f9-4979w   1/1     Running   0          13m
pod/httpenv-844f545f9-8kp7t   1/1     Running   0          13m
pod/httpenv-844f545f9-8qvb9   1/1     Running   0          13m
pod/httpenv-844f545f9-ss6sn   1/1     Running   0          17m
pod/httpenv-844f545f9-tznmt   1/1     Running   0          13m
pod/tmp-shell                 1/1     Running   0          9m26s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/httpenv      ClusterIP   10.100.88.68   <none>        8888/TCP         12m
service/httpenv-np   NodePort    10.96.14.27    <none>        8888:30225/TCP   47s
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          23h

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/httpenv   5/5     5            5           17m

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/httpenv-844f545f9   5         5         5       17m
```

Default range preset in your docker container cluster - 30 000 - 32 767

- Did you know that a NodePort service also creates a ClusterIP?
- These threee service types are additive, each one creates the ones above it
  - ClusterIP
  - NodePort
  - LoadBalancer

Important: You can access it: `http://localhost:30225/`

`Paton@DESKTOP-L9OMUGO MINGW64 ~
$ curl http://localhost:30225/`

```
{"HOME":"/root","HOSTNAME":"httpenv-844f545f9-8qvb9","KUBERNETES_PORT":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP_ADDR":"10.96.0.1","KUBERNETES_PORT_443_TCP_PORT":"443","KUBERNETES_PORT_443_TCP_PROTO":"tcp","KUBERNETES_SERVICE_HOST":"10.96.0.1","KUBERNETES_SERVICE_PORT":"443","KUBERNETES_SERVICE_PORT_HTTPS":"443","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"}
```

## Add a LoadBalancer Service

If you are on Docker Desktop, it provides a built-ion LoadBalancer that publishes the `--port` on localhost - `kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer` - `curl localhost:8888`

`$ kubectl get services`

```
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
httpenv      ClusterIP      10.100.88.68   <none>        8888/TCP         20m
httpenv-lb   LoadBalancer   10.106.16.36   localhost     8888:31468/TCP   19s
httpenv-np   NodePort       10.96.14.27    <none>        8888:30225/TCP   9m1s
kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP          23h
```

- If you are on kubeadm, minikube, or microk8s
  - no built-in lb
  - you can still run the command, it will just stay at "pending" (but its nodePort works)
- one of the way to have your kubernetes expose on specific port - nice!

## cleanup

- Lets remove the services and deploymetn
  - `kubectl delete service/httpenv service/httpenv-up`
  - `kubectl delete service/httpenv-lb deployment/httpenv`
