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
- create/update a whole directory of yaml
    - `create/update -f myyaml/`
-create/update from a URL`
    - `kubectl apply -f https://bret.run/pod.yml`
- Ba careful, lets look at it first (browser or curl)
    - `curl -L https://bret.run/pod`
    - `Win PoSH? start https://bret.run/pod.yml`

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
    
