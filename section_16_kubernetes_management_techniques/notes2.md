# 123 The Future of Kubectl Run

Link:

- https://kubernetes.io/docs/reference/kubectl/conventions/#best-practices

- 1.12 - 1.15 run is in the a state of flux
- the goals is to reduce its features to only create pods
  - right now it defaults to creating deployments (with the warning)
  - it has lots of generators but they are all deprecated
  - The idea is to make it easy like `docker run` for one-off tasks
  - it is going to be created only to create onetime run engines etc
- it is not recommended for production - one shot container
  - network testing
  - use for simple dev/test or troubleshooting pods

## Old Run Confusion ( I am using 1.25 so it is creating only pods )

    - the generators activate different Controllers based on options
    - using dry-run we can see which generators are used
        - `kubectl run test --image nginx --dry-run`
        - `kubectl run test --image nginx --port 90 --expose --dry-run=client`
        - `kubectl run test --image nginx --restart OnFailure --dry-run=client`
        - `kubectl run test --image nginx --restart Never --dry-run=client`
        - `kubectl run test --image nginx --schedule "*/1 * * * *" --dry-run=client`   (in old version it would be chrome job etc)

# 124 Imperative vs Declarative
    - Imperative: Focus on How a program operates
    - Declarative: Focus on What a program should accomplis
        - Example: "I'd like a cup of coffee"
        - imperative: I boil water scoop out 42 grams of medium-fine grounds, poor over 700 grams of water etc.
        - declarative: "Barista, I'd like a cup of a coffee
            (Barista is the engine that works though the steps, including retrying to make a cup, and is only finished when I have a cup)

## Kubernetes Imperative
    - Examples: kubectl run, kubectl create deployment, kubectl update
        - we start with a state we know (no deployment exists)
        - we ask kubectl run to create a deployment
    - different commands are required to change that deployment
    - different commands are required per object
    - imperative is easier when you know the state
    - imperative is easier to get started
    - imperative is easier for humans at the cli
    - imperative is not easy to automate!

## Kubernetes Declarative
    - Example: kubectl apply -f my-resources.yaml
        - we do not know the current state
        - we only know what we want the end result to be (yaml contents)
    - same command each time (tiny exception for delete)
    - resources can be all in a file, or many files (apply a whole dir)
    - requires understanding the yaml keys and values
    - more work than kubectl run for just starting a pod
    - the easiest way to automate
    - the eventual path to GitOps happiness



