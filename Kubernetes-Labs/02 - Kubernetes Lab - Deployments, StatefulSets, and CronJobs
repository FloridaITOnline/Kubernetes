# Kubernetes Lab 02 - Deployments, StatefulSets, and CronJobs

## Overview

This lab introduces Kubernetes workload controllers and demonstrates when to use each one based on application requirements.

Rather than creating Pods directly, Kubernetes provides higher-level controllers that automatically manage Pods, scaling, updates, scheduling, and recovery.

During this lab the following Kubernetes objects were created:

- Deployment
- StatefulSet
- CronJob

---

# Prerequisites

Before completing this lab, a functional Kubernetes cluster must already exist.

This repository's previous lab covers building the cluster from scratch using `kubeadm`.

The cluster should already include:

- Ubuntu Linux
- containerd
- kubeadm
- kubelet
- kubectl
- Initialized Control Plane
- Calico CNI installed
- Worker nodes joined to the cluster

Verify the environment before beginning:

```bash
kubectl version --short
kubectl cluster-info
kubectl get nodes -o wide
kubectl get pods -A
```

---

# Learning Objectives

- Deploy stateless applications
- Deploy stateful applications
- Schedule recurring workloads
- Learn when to use each Kubernetes controller

---

# Deployment

## Use Case

Deployments manage stateless applications.

Features include:

- Rolling updates
- Zero downtime deployments
- Easy horizontal scaling
- Self-healing Pods

### Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:stable
```

Deploy:

```bash
kubectl apply -f deployment.yaml
```

Verify:

```bash
kubectl get deployments
kubectl get pods
```

---

# StatefulSet

## Use Case

StatefulSets are designed for applications that require persistent identity.

Examples include:

- PostgreSQL
- MySQL
- MongoDB
- Elasticsearch
- Kafka

Features include:

- Predictable Pod names
- Ordered startup
- Ordered shutdown
- Stable network identity

### StatefulSet YAML

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: legacy
spec:
  serviceName: legacy
  replicas: 5
  selector:
    matchLabels:
      app: legacy
  template:
    metadata:
      labels:
        app: legacy
    spec:
      containers:
      - name: nginx
        image: nginx:stable
```

Deploy:

```bash
kubectl apply -f stateful.yaml
```

Verify:

```bash
kubectl get statefulsets
kubectl get pods
```

Expected Pod names:

```
legacy-0
legacy-1
legacy-2
legacy-3
legacy-4
```

> **Note:** Production StatefulSets generally require a Headless Service (`clusterIP: None`). This training lab did not require one.

---

# CronJob

## Use Case

CronJobs execute Jobs on a schedule.

Examples include:

- Database backups
- Log cleanup
- Report generation
- Maintenance tasks

### CronJob YAML

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: data-cleanup
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cleanup
            image: busybox:stable
            command:
            - date
          restartPolicy: OnFailure
```

Deploy:

```bash
kubectl apply -f cronjob.yaml
```

Verify:

```bash
kubectl get cronjobs
kubectl get jobs
kubectl get pods
```

---

# Useful Commands

Check workloads:

```bash
kubectl get deployments
kubectl get statefulsets
kubectl get cronjobs
kubectl get jobs
kubectl get pods
```

Describe resources:

```bash
kubectl describe deployment web

kubectl describe statefulset legacy

kubectl describe cronjob data-cleanup
```

View running configuration:

```bash
kubectl get deployment web -o yaml

kubectl get statefulset legacy -o yaml
```

---

# Troubleshooting

## Deployment Warning

If a Deployment was originally created with:

```bash
kubectl create deployment web --image=nginx:stable
```

then applying YAML later may display:

```
resource is missing the last-applied-configuration annotation
```

This warning is expected and does not indicate a failure.

---

## StatefulSet Validation Error

Example:

```
unknown field "spec" in LabelSelector
missing required field "template"
```

Cause:

The Pod template was accidentally nested beneath the selector.

Incorrect:

```yaml
selector:
  matchLabels:
    app: legacy
  spec:
```

Correct:

```yaml
selector:
  matchLabels:
    app: legacy

template:
  metadata:
    labels:
      app: legacy
  spec:
```

YAML indentation is critical.

---

# Workload Comparison

| Controller | Primary Use Case |
|------------|------------------|
| Deployment | Stateless applications |
| StatefulSet | Stateful applications requiring stable identity |
| DaemonSet | One Pod on every Node |
| Job | Run once until completion |
| CronJob | Run Jobs on a schedule |

---

# Key Takeaways

One of the most important Kubernetes design decisions is selecting the appropriate workload controller.

- Use **Deployments** for stateless applications that need rolling updates and horizontal scaling.
- Use **StatefulSets** for workloads requiring predictable identities or persistent storage.
- Use **CronJobs** for scheduled maintenance or automation tasks.
- Avoid creating standalone Pods unless testing or debugging.

Understanding these controllers is foundational for administering production Kubernetes clusters.
