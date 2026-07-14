# 03 - Kubernetes Workload Controllers Lab

> **Repository:** FLITO Kubernetes Labs\
> **Topic:** Deployments, StatefulSets, and CronJobs

## Overview

This lab demonstrates three of the most common Kubernetes workload
controllers:

-   **Deployment** -- Stateless applications that support scaling and
    rolling updates.
-   **StatefulSet** -- Stateful workloads that require stable Pod
    identities.
-   **CronJob** -- Scheduled tasks that execute to completion.

------------------------------------------------------------------------

# Deployment

## When to Use

Use a Deployment for stateless applications such as:

-   Web applications
-   REST APIs
-   Microservices
-   Background workers

### Example

``` yaml
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

``` bash
kubectl apply -f web.yml
```

Verify:

``` bash
kubectl get deployment web
kubectl get pods
```

Example Pods:

    web-7d8d4f79b5-abc12
    web-7d8d4f79b5-def34
    web-7d8d4f79b5-ghi56

------------------------------------------------------------------------

# StatefulSet

## When to Use

Use a StatefulSet when Pods require:

-   Stable identities
-   Predictable names
-   Ordered startup and shutdown
-   Persistent storage (when applicable)

Examples include:

-   PostgreSQL
-   MySQL
-   MongoDB
-   Redis Cluster
-   Kafka

A StatefulSet is typically paired with a **headless Service**.

### Example

``` yaml
apiVersion: v1
kind: Service
metadata:
  name: statefulset-service
spec:
  clusterIP: None
  selector:
    app: statefulset-demo
  ports:
  - name: http
    port: 80

---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: statefulset-demo
spec:
  serviceName: statefulset-service
  replicas: 5
  selector:
    matchLabels:
      app: statefulset-demo
  template:
    metadata:
      labels:
        app: statefulset-demo
    spec:
      containers:
      - name: nginx
        image: nginx:stable
```

Deploy:

``` bash
kubectl apply -f statefulset-demo.yml
```

Verify:

``` bash
kubectl get statefulsets
kubectl get pods
```

Example Pods:

    statefulset-demo-0
    statefulset-demo-1
    statefulset-demo-2
    statefulset-demo-3
    statefulset-demo-4

Unlike a Deployment, these Pod names remain consistent if a Pod is
recreated.

------------------------------------------------------------------------

# CronJob

## When to Use

Use a CronJob for scheduled tasks such as:

-   Backups
-   Cleanup jobs
-   Report generation
-   Cache refresh
-   Scheduled scripts

### Example

``` yaml
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
          - name: busybox
            image: busybox:stable
            command:
            - date
          restartPolicy: OnFailure
```

Deploy:

``` bash
kubectl apply -f data-cleanup.yml
```

Verify:

``` bash
kubectl get cronjobs
kubectl get jobs
kubectl get pods
```

------------------------------------------------------------------------

# Architecture Overview

    Deployment
        │
        ▼
    ReplicaSet
        │
     ┌──┴──┐
     ▼     ▼
    Pod   Pod

    StatefulSet
        │
        ▼
    statefulset-demo-0
    statefulset-demo-1
    statefulset-demo-2
    statefulset-demo-3
    statefulset-demo-4

    CronJob
        │
        ▼
    Job
        │
        ▼
    Pod

------------------------------------------------------------------------

# Useful Commands

``` bash
kubectl get all
kubectl describe deployment web
kubectl describe statefulset statefulset-demo
kubectl describe cronjob data-cleanup
kubectl scale deployment web --replicas=5
kubectl rollout status deployment web
```

------------------------------------------------------------------------

# KCNA / CKA Study Notes

  Controller    Best Use Case
  ------------- ------------------------------------
  Deployment    Stateless applications
  StatefulSet   Stable identities and ordered Pods
  Job           Run once
  CronJob       Scheduled execution
  DaemonSet     One Pod per node

------------------------------------------------------------------------

## Key Takeaways

-   Deployments provide rolling updates and horizontal scaling.
-   StatefulSets provide predictable Pod names and stable identities.
-   CronJobs execute Jobs on a recurring schedule.
-   Labels and selectors connect controllers to Pods.
-   Kubernetes continuously reconciles the cluster to the desired state.
