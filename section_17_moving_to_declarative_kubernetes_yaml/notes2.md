# 130 Building Your YAML spec

- We can get all the keys each kind supports
  - `kubectl explain services --recursive`

```
KIND:     Service
VERSION:  v1

DESCRIPTION:
     Service is a named abstraction of software service (for example, mysql)
     consisting of local port (for example 3306) that the proxy listens on, and
     the selector that determines which pods will answer requests sent through
     the proxy.

FIELDS:
   apiVersion   <string>
   kind <string>
   metadata     <Object>
      annotations       <map[string]string>
      creationTimestamp <string>
      deletionGracePeriodSeconds        <integer>
      deletionTimestamp <string>
      finalizers        <[]string>
      generateName      <string>
      generation        <integer>
      labels    <map[string]string>
```

- oh boy! lets slow down hahaha xD
  - `kubectl explain services.spec`

```
KIND:     Service
VERSION:  v1

RESOURCE: spec <Object>

DESCRIPTION:
     Spec defines the behavior of a service.
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

     ServiceSpec describes the attributes that a user creates on a service.

FIELDS:
   allocateLoadBalancerNodePorts        <boolean>
     allocateLoadBalancerNodePorts defines if NodePorts will be automatically
     allocated for services with type LoadBalancer. Default is "true". It may be
     set to "false" if the cluster load-balancer does not rely on NodePorts. If
     the caller requests specific NodePorts (by specifying a value), those
     requests will be respected, regardless of this field. This field may only
     be set for services with type LoadBalancer and will be cleared if the type
     is changed to any other type.

   clusterIP    <string>
     clusterIP is the IP address of the service and is usually assigned
     randomly. If an address is specified manually, is in-range (as per system
     configuration), and is not in use, it will be allocated to the service;
```

- We can walk through the spec this way

  - `kubectl explain services.spec.type`

- spec: can have sub spec: of other resources - `kubectl explain deployment.spec.template.spec.volumes.nfs.server`
  -we can also use docs  
   - Link: https://kubernetes.io/docs/reference/#api-reference

---

# 131 Dry Runs and Diff's
## Testing YAML before deployment

    - dry-run a create (client side only)
        - `kubectl apply -f app.yml --dry-run-client`
    - dry-run a create/update on server
        - `kubectl apply -f app.yml --dry-run=server`
    - see a diff visually
        - kubectl diff -f app.yml

`$ kubectl diff -f app.yml`
```
diff -u -N "C:\\Users\\Paton\\AppData\\Local\\Temp\\LIVE-2183679634/apps.v1.Deployment.default.app-nginx-deployment" "C:\\Users\\Paton\\AppData\\Local\\Temp\\MERGED-1781886807/apps.v1.Deployment.default.app-nginx-deployment"
--- "C:\\Users\\Paton\\AppData\\Local\\Temp\\LIVE-2183679634/apps.v1.Deployment.default.app-nginx-deployment"   2023-03-16 06:11:58.132385200 +0000
+++ "C:\\Users\\Paton\\AppData\\Local\\Temp\\MERGED-1781886807/apps.v1.Deployment.default.app-nginx-deployment" 2023-03-16 06:11:58.135784200 +0000
@@ -6,14 +6,16 @@
     kubectl.kubernetes.io/last-applied-configuration: |
       {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"app-nginx-deployment","namespace":"default"},"spec":{"replicas":3,"selector":{"matchLabels":{"app":"app-nginx"}},"template":{"metadata":{"labels":{"app":"app-nginx"}},"spec":{"containers":[{"image":"nginx:1.17.3","name":"nginx","ports":[{"containerPort":80}]}]}}}}
   creationTimestamp: "2023-03-16T06:09:15Z"
-  generation: 1
+  generation: 2
+  labels:
+    servers: dmz
   name: app-nginx-deployment
   namespace: default
   resourceVersion: "43375"
   uid: 72eab3e5-94c5-479b-b931-918495f02a25
 spec:
   progressDeadlineSeconds: 600
-  replicas: 3
+  replicas: 2
   revisionHistoryLimit: 10
   selector:
     matchLabels:
```

https://kubernetes.io/blog/2019/01/14/apiserver-dry-run-and-kubectl-diff/

