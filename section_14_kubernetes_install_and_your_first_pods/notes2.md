# 107 Kubectl, create and apply

- Kubernetes is evolving and so is the CLI
- We get three ways to create pods from the kubectl CLI
  - `kubectl run` (single pod per command since 1.18)
  - `kubectl create` (create some resources via CLI or YAML)
  - `kubectl apply` (create/update anything via YAML)
- For now we will just use `run` or `create` CLI
- Later we will learn YAML and pros/cons of each
