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