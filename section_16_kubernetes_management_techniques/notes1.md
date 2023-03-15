# 121 Section Intro

- Kubectl Generators
- The future of Kubectl Run
- Imperative vs Declarative
- Three Management Approaches

# 122 Run, Expose and Create Generators

- These commands use helper templates called "Generators"
- every resource in Kubernetes has a specification or 'spec'
  - `kubectl create deployment sample --image nginx --dry-run -o yaml`
    - (swarm is doing the same)
- you can output those templates with `--dry-run -o yaml`
- you can use output to start creating your yaml
- Generators are 'opinionated defaults"

## Generator Examples

- Using dry-run with yaml output we can see the generators
  - `kubectl create deployment test --image nginx --dry-run -o yaml`
  - `kubectl create deployment test --image nginx --dry-run=client -o yaml`
  - `kubectl create job test --image nginx --dry-run=client -o yaml`

```
W0315 06:45:11.396013   20920 helpers.go:663] --dry-run is deprecated and can be replaced with --dry-run=client.
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:               <- names
    app: sample
  name: sample
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sample
  strategy: {}
  template:             <- replica set
    metadata:
      creationTimestamp: null
      labels:
        app: sample
    spec:               <- container that is going to be created
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

`$ kubectl create job test --image nginx --dry-run=client -o yaml`

```
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: test
spec:
  template:
    metadata:
      creationTimestamp: null
    spec:                       <- just container
      containers:
      - image: nginx
        name: test
        resources: {}
      restartPolicy: Never      <- never because when job is finished it would be removed
status: {}
```

`$ kubectl expose deployment/test --port 80 --dry-run=client -o yaml`
You needed to create deployment

```
$ kubectl create deployment test --image nginx
deployment.apps/test created
```

`$ kubectl expose deployment/test --port 80 --dry-run=client -o yaml`

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: test
  name: test
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: test
status:
  loadBalancer: {}
```
