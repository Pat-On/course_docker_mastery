# 126 Section Intro

- Kubectl apply
- k8s configuration YAML
- Building your yaml files
- building your yaml spec
- dry runs and diffs
- labels and annotation

# 127 Kubectl apply

Read more: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/

- Remember the three management approaches?
- lets skip to full declarative objects
  - `kubectl apply -f filename.yml`
  - why skip `kubectl create`, `kubectl replace` etc...
- what I recommend =/= all that is possible

## Using kubectl apply

- create/update resource in a file
  - `kubectl apply -f myfile.yaml`
- create/update a whole directory of yaml - `create/update -f myyaml/`
  -create/update from a URL`    -`kubectl apply -f https://bret.run/pod.yml`
- Ba careful, lets look at it first (browser or curl)
  - `curl -L https://bret.run/pod`
  - `Win PoSH? start https://bret.run/pod.yml`

---

# 128 Kubernetes Configuration YAML

- Kubernetes Configuration file (yaml or JSON)
  - yaml is going to be changed to json - computers are better in reading it
- each file contains one or more manifests
- each manifest describes an API object (deployment, job, secret)
- each manifest needs four parts (root key:values in the files)
  - apiVersion:
  - kind:
  - metadata:
  - spec:

---

# 129 Building Your YAML Files

- `kind:` We canm get a list of resources the cluster supports
  - `kubectl api-resources`

```
NAME                  SHORTNAMES   APIVERSION     NAMESPACED        KIND
bindings                               v1             true         Binding
componentstatuses          cs          v1             false        ComponentStatus
configmaps                 cm          v1             true         ConfigMap
endpoints                  ep          v1             true         Endpoints
```

Kind - used in yaml

- notice some resources have multiple API's (old vs new)
- `apiVersion`: we can get the API versions the cluster supports
  - `kubectl api-versions`

```
admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1
apiregistration.k8s.io/v1
apps/v1
authentication.k8s.io/v1
authorization.k8s.io/v1
autoscaling/v1
autoscaling/v2
autoscaling/v2beta2
batch/v1
certificates.k8s.io/v1
coordination.k8s.io/v1
discovery.k8s.io/v1
events.k8s.io/v1
flowcontrol.apiserver.k8s.io/v1beta1
flowcontrol.apiserver.k8s.io/v1beta2
networking.k8s.io/v1
node.k8s.io/v1
policy/v1
rbac.authorization.k8s.io/v1
scheduling.k8s.io/v1
storage.k8s.io/v1
storage.k8s.io/v1beta1
v1
```

So:

```apiVersion: extension/v1beta1
kind: Deployment
metadata:
spec:```
                            depreciation 
```apiVersion: apps/v1
kind: Deployment
metadata:
spec:```

The best way of checking it is when you are taking old file and you are having error

kind + apiVerison is used to decide what resource kind we are going to use and apiVersion

- `metadata:` only name is required
- `spec:` Where all the action is at! 

