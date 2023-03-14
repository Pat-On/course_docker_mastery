# 110 Your First Deployment With kubectl create

## creating a Deployment with kubectl

- Lets create a Deployment of the nginx web server!

  - `kubectl create deployment my-nginx --image nginx`
    (we use run more for testing and just check deployment gives us all the features foir production)

- lets list the pod
  - `kubectl get pods`

`kubectl get pods`

```
NAME                       READY   STATUS    RESTARTS        AGE
my-nginx                   1/1     Running   1 (5m34s ago)   14h
my-nginx-f44495498-svtfm   1/1     Running   0               49s   - created using deplyment
```

`kubectl get all`

```
NAME                           READY   STATUS    RESTARTS        AGE
pod/my-nginx                   1/1     Running   1 (6m23s ago)   14h
pod/my-nginx-f44495498-svtfm   1/1     Running   0               98s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   14h

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx   1/1     1            1           98s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-f44495498   1         1         1       98s
```

deployment -> api (standard web api) --> ETCD (store record for deployment) -> cm (control manager - noticed new resource type - pod and deployment)

Deployment -> ReplicaSet -> Pods

## cleanup

- Lets remove the single pod and the deployment
  - `kubectl delete pod my-nginx`
  - `kubectl delete deployment my-nginx`

---

# 111 Scaling ReplicaSets

- start a new deployment for one replica/pod

  - `kubectl create deployment my-apache --image httpd`

- Check our resource status
  - `kubectl get all`

`$ kubectl get all`

```
NAME                             READY   STATUS    RESTARTS   AGE
pod/my-apache-86c695c9b6-9pmqf   1/1     Running   0          16s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   15h

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-apache   1/1     1            1           16s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/my-apache-86c695c9b6   1         1         1       16s
```

- lets scale it up with another pod
  - `kubectl scale deploy/my-apache --replicas 2`
  - `kubectl scale deploy my-apache --replicas 2`
  - those are the same command
  - deploy = deployment = deployments

```
NAME                             READY   STATUS              RESTARTS   AGE
pod/my-apache-86c695c9b6-9pmqf   1/1     Running             0          3m22s
pod/my-apache-86c695c9b6-vf7rw   0/1     ContainerCreating   0          3s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   15h

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-apache   1/2     2            1           3m22s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/my-apache-86c695c9b6   2         2         1       3m22s
```

```
NAME                             READY   STATUS    RESTARTS   AGE
pod/my-apache-86c695c9b6-9pmqf   1/1     Running   0          3m26s
pod/my-apache-86c695c9b6-vf7rw   1/1     Running   0          7s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   15h

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-apache   2/2     2            2           3m26s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/my-apache-86c695c9b6   2         2         2       3m26s
```

## What happened when we scaled?

- kubectl scale will _change_ the deployment/my-apache record
- cm will see that _only_ replica count has changed
- it will change the number of pods in ReplicaSet
- Scheduler sees a new pod is requested, assigns a node
- Kubelet sees as new pod, tells container runtime to start httpd

---

# Inspecting Deployment Objects

- kubectl get pods
- get containers logs

`kubectl logs deployment/my-apache`

```
Found 2 pods, using pod/my-apache-86c695c9b6-9pmqf
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.0.17. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.0.17. Set the 'ServerName' directive globally to suppress this message
[Tue Mar 14 21:40:20.035909 2023] [mpm_event:notice] [pid 1:tid 140322113596736] AH00489: Apache/2.4.56 (Unix) configured -- resuming normal operations
[Tue Mar 14 21:40:20.037723 2023] [core:notice] [pid 1:tid 140322113596736] AH00094: Command line: 'httpd -D FOREGROUND'
```

- `kubectl logs deployment/my-apache --follow --tail 1`

- Get a bunch of details about an object, including events!
  - `kubectl logs -l run=my-apache`

```no clue why
$ kubectl logs -l run=my-apache
No resources found in default namespace.
```

    - `kubectl describe pod/my-apache-xxxx-yyyy`

` kubectl describe pod/my-apache-86c695c9b6-9pmqf`
```
Name:             my-apache-86c695c9b6-9pmqf
Namespace:        default
Priority:         0
Service Account:  default
Node:             docker-desktop/192.168.65.4
Start Time:       Tue, 14 Mar 2023 21:40:15 +0000
Labels:           app=my-apache
                  pod-template-hash=86c695c9b6
Annotations:      <none>
Status:           Running
IP:               10.1.0.17
IPs:
  IP:           10.1.0.17
Controlled By:  ReplicaSet/my-apache-86c695c9b6
Containers:
  httpd:
    Container ID:   docker://5a479c9a26b525e30bb84375fb136e7c7bc129c75035c40d27d382512cd486b0
    Image:          httpd
    Image ID:       docker-pullable://httpd@sha256:76618ddd53f315a1436a56dc84ad57032e1b2123f2f6489ce9c575c4b280c4f4
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 14 Mar 2023 21:40:19 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-49w49 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  kube-api-access-49w49:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  17m   default-scheduler  Successfully assigned default/my-apache-86c695c9b6-9pmqf to docker-desktop
  Normal  Pulling    17m   kubelet            Pulling image "httpd"
  Normal  Pulled     17m   kubelet            Successfully pulled image "httpd" in 2.00424103s
  Normal  Created    17m   kubelet            Created container httpd
  Normal  Started    17m   kubelet            Started container httpd
```

---

## prove that kubernetes is very resilient
- watch a command (without needing watch)
    - `kubectl get pods -w`
- in a separate tab/window
    - `kubectl delete pod/<pod name>
- watch the pod get re-created

to clean:
    kubectl delete deployment my-apache