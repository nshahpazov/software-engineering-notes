---
title: Notes on Docker and Kubernetes and EKS
category: Kubernetes & Containers
tags: [docker, kubernetes, minikube, eks, k8s]
description: Local Kubernetes workflow with Minikube, building images, services and scaling.
status: notes
---

# Notes on Docker and Kubernetes and EKS

1. Install kubectl

```bash
brew install kubectl
```
2. Install minikube

```bash
brew install minikube
```

- kubectl get nodes # This command lists the nodes in the Kubernetes cluster.
- kubectl get pods # This command lists the pods in the Kubernetes cluster.

The difference between pods and nodes:
- **Pods** are the smallest deployable units in Kubernetes, which can contain one or more containers.
- **Nodes** are the machines (physical or virtual) that run the pods. Each node can host multiple pods.


When istalling minikube, you provide a second Docker engine with it, apart from the Orbstack Docker engine.
To switch to the minikube Docker engine, you can use the following command:

```bash
eval $(minikube docker-env)
```

If you want to switch back to the Orbstack Docker engine, you can use:

```bash
eval $(minikube docker-env --unset)
```

Confirm the root dir of the Docker engine with:

```bash
which docker
docker info | grep "Docker Root Dir"
```

Building the image is happening with

```bash
docker build -t hello-node .
```

To load an image to the minikube Docker engine, you can use the following command:
```bash
minikube image load hello-node
```

After that we can apply a Kubernetes deployment with:

```bash
kubectl apply -f deployment.yaml
```

Example deployment.yaml file:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-node
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-node
  template:
    metadata:
      labels:
        app: hello-node
    spec:
      containers:
      - name: hello-node
        image: hello-node  # no registry, just local name
        imagePullPolicy: Never
        ports:
        - containerPort: 3000

```

A deployment is a Kubernetes resource that manages a set of identical pods, ensuring that the specified number of replicas are running at all times.
To expose the deployment to the outside world using minikube we can use:

```bash
minikube service hello-nodekubectl scale deployment hello-node --replicas=3
```
This command creates a service that routes traffic to the pods created by the deployment.
In Kubernetes, a Service is a method for exposing a network application that is running as one or more Pods in your cluster.
To scale the deployment to 3 replicas, you can use:

```bash
kubectl scale deployment hello-node --replicas=3
```


To delete a service, you can use:

```bash
kubectl delete service hello-node
```

To expose a deployment to the outside world, you can use:

```bash
kubectl expose deployment hello-node --type=LoadBalancer --port=3001
```

After that we tunnel the service to our local machine with:

```bash
minikube service hello-node
```

```
[1] Build Docker Image (inside Minikube)
    ┌────────────────────────────────┐
    │ eval $(minikube docker-env)    │
    │ docker build -t hello-node .   │
    └────────────────────────────────┘

[2] Apply Kubernetes Deployment
    ┌────────────────────────────────────────────┐
    │ kubectl apply -f deployment.yaml           │
    │ (image: hello-node, imagePullPolicy: Never)│
    └────────────────────────────────────────────┘

[3] Expose Deployment via Service
    ┌────────────────────────────────────────────┐
    │ kubectl expose deployment hello-node       │
    │ --type=LoadBalancer --port=3000            │
    └────────────────────────────────────────────┘

[4] Access App via Minikube Tunnel
    ┌────────────────────────────────────────────┐
    │ minikube service hello-node                │
    │ → Opens browser to local running service   │
    └────────────────────────────────────────────┘
```


```
Browser on host
    ↓
Tunnel created by Minikube
    ↓
Service inside K8s
    ↓
Pod with your app
```


This command will open a browser window with the service running on your local machine, allowing you to access the application.
Since usually we use a cloud environment with a load balancer, we need to use a tunnel to access the service locally.

Getting the list of pods in the minikube cluster can be done with:
```bash
kubectl get pods -o wide
```
This shows additional information like the node the pod is running on.