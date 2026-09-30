# kubernetes-production-project

# Kubernetes Foundations Project

## Overview

This project is a beginner-friendly introduction to Kubernetes.

The goal is to learn how Kubernetes deploys, manages, scales, and maintains containerized applications using core Kubernetes resources such as Deployments, Pods, Services, ConfigMaps, and Namespaces.

In this project, I deployed an Nginx web application to a local Kubernetes cluster using Minikube and managed it using `kubectl`.

## Technologies Used

- Kubernetes
- Minikube
- kubectl
- Nginx
- YAML
- Docker

## Project Structure

```text
kubernetes-foundations/
├── namespace.yaml
├── configmap.yaml
├── deployment.yaml
├── service.yaml
└── README.md
```

## Kubernetes Resources

### Namespace

A Namespace was created to logically separate the project resources from other workloads in the Kubernetes cluster.

```text
k8s-foundations
```

### ConfigMap

A ConfigMap stores the HTML content displayed by the Nginx web server.

This demonstrates how application configuration can be separated from the container image.

### Deployment

The Deployment manages the Nginx application Pods.

The application initially runs with:

```text
2 replicas
```

Kubernetes automatically maintains the required number of Pods.

### Service

A Kubernetes `NodePort` Service exposes the Nginx application and routes traffic to Pods with the matching label.

## Deploy the Project

Start Minikube:

```bash
minikube start
```

Verify the cluster:

```bash
kubectl get nodes
```

Deploy the Kubernetes resources:

```bash
kubectl apply -f namespace.yaml
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Alternatively:

```bash
kubectl apply -f .
```

## Verify the Deployment

Check the Pods:

```bash
kubectl get pods -n k8s-foundations
```

Check the Deployment:

```bash
kubectl get deployments -n k8s-foundations
```

Check the Service:

```bash
kubectl get services -n k8s-foundations
```

View all project resources:

```bash
kubectl get all -n k8s-foundations
```

## Access the Application

Using Minikube:

```bash
minikube service web-service -n k8s-foundations
```

Alternatively, use port forwarding:

```bash
kubectl port-forward service/web-service 8080:80 -n k8s-foundations
```

Then open:

```text
http://localhost:8080
```

## Scaling the Application

Scale the Deployment to five Pods:

```bash
kubectl scale deployment web-app \
  --replicas=5 \
  -n k8s-foundations
```

Verify:

```bash
kubectl get pods -n k8s-foundations
```

This demonstrates Kubernetes horizontal scaling.

## Testing Self-Healing

List the running Pods:

```bash
kubectl get pods -n k8s-foundations
```

Delete one Pod:

```bash
kubectl delete pod <pod-name> -n k8s-foundations
```

Watch the Pods:

```bash
kubectl get pods -n k8s-foundations -w
```

Kubernetes automatically creates a replacement Pod because the Deployment maintains the desired replica count.

## Rolling Updates

Update the Nginx image:

```bash
kubectl set image \
  deployment/web-app \
  nginx=nginx:mainline-alpine \
  -n k8s-foundations
```

Monitor the rollout:

```bash
kubectl rollout status deployment/web-app -n k8s-foundations
```

View rollout history:

```bash
kubectl rollout history deployment/web-app -n k8s-foundations
```

## Rollback

If a deployment causes problems, roll back to the previous version:

```bash
kubectl rollout undo deployment/web-app -n k8s-foundations
```

## Troubleshooting

Useful commands used during the project:

```bash
kubectl get pods -n k8s-foundations
```

```bash
kubectl describe pod <pod-name> -n k8s-foundations
```

```bash
kubectl logs <pod-name> -n k8s-foundations
```

```bash
kubectl get events -n k8s-foundations
```

```bash
kubectl get endpoints web-service -n k8s-foundations
```

Common issues explored included:

- `ImagePullBackOff`
- Incorrect container image tags
- Service selector and Pod label mismatches
- Pods failing to start
- Application connectivity problems

## Key Concepts Learned

This project helped reinforce the following Kubernetes concepts:

- Pods
- Deployments
- ReplicaSets
- Services
- Namespaces
- ConfigMaps
- Labels and selectors
- Desired state
- Self-healing
- Horizontal scaling
- Rolling updates
- Rollbacks
- Basic Kubernetes troubleshooting

## Architecture

```text
User
 │
 ▼
Kubernetes Service
 │
 ▼
Deployment
 │
 ▼
ReplicaSet
 │
 ├── Pod
 │    └── Nginx
 │
 └── Pod
      └── Nginx
```

## Conclusion

This project provided hands-on experience with the core building blocks of Kubernetes.

By deploying, scaling, updating, breaking, troubleshooting, and restoring a simple web application, I gained a better understanding of how Kubernetes maintains application availability and manages containerized workloads.