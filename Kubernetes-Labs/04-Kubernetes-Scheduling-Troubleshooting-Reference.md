# Kubernetes Scheduling Troubleshooting Reference

> **Repository:** FLITO Kubernetes Labs\
> **Category:** Troubleshooting & kubectl Reference

------------------------------------------------------------------------

# Overview

Scheduling is the process Kubernetes uses to determine **which Node will
run a Pod**.

When a Pod cannot be placed on a Node, it typically remains in the
**Pending** state. Fortunately, Kubernetes provides several commands
that make it easy to determine where the scheduling process is failing.

This guide focuses on the commands commonly used during scheduling
troubleshooting and explains how they relate to your Kubernetes
manifests.

------------------------------------------------------------------------

# Troubleshooting Workflow

When investigating scheduling issues, start broad and work toward the
specific problem.

``` text
kubectl get deployments
        │
        ▼
kubectl get pods
        │
        ▼
kubectl describe pod <pod-name>
        │
        ▼
kubectl get nodes
        │
        ▼
Review the Deployment YAML
        │
        ▼
Correct the manifest
        │
        ▼
kubectl apply -f <manifest>.yml
```

------------------------------------------------------------------------

# kubectl get deployments

``` bash
kubectl get deployments
```

## Purpose

Displays all Deployments in the current namespace.

Example:

``` text
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
web       3/3     3            3           18m
api       1/1     1            1           2d
```

## Understanding the Output

  Column       Description
  ------------ -------------------------------------------------
  NAME         Deployment name
  READY        Running replicas / Desired replicas
  UP-TO-DATE   Pods using the current Deployment specification
  AVAILABLE    Pods available to receive traffic
  AGE          How long the Deployment has existed

## When to Use

Use this command first to determine whether the Deployment itself
appears healthy.

If READY is less than the desired replica count, continue with:

``` bash
kubectl get pods
```

------------------------------------------------------------------------

# kubectl get pods

``` bash
kubectl get pods
```

## Purpose

Displays every Pod in the current namespace.

Example:

``` text
NAME                      READY   STATUS
web-6f8d9c7d5f-abc12      1/1     Running
web-6f8d9c7d5f-def34      0/1     Pending
```

## Common Pod Status Values

  Status             Meaning
  ------------------ --------------------------------------------------
  Running            Pod is healthy and running
  Pending            Waiting to be scheduled or waiting for resources
  CrashLoopBackOff   Container repeatedly crashes
  ImagePullBackOff   Image cannot be downloaded
  Completed          Job or CronJob completed successfully

## When to Use

This command identifies which Pods are experiencing problems.

If a Pod is Pending or repeatedly restarting, inspect it with:

``` bash
kubectl describe pod <pod-name>
```

------------------------------------------------------------------------

# kubectl describe pod

``` bash
kubectl describe pod <pod-name>
```

## Purpose

Displays detailed information about a specific Pod.

Useful sections include:

-   Node assignment
-   Labels
-   Container image
-   Mounted volumes
-   Conditions
-   Events

## Why It Matters

The **Events** section often contains the exact reason Kubernetes could
not schedule or start the Pod.

Examples include:

-   FailedScheduling
-   FailedMount
-   FailedPull
-   Back-off restarting container

## When to Use

Whenever a Pod is not behaving as expected.

------------------------------------------------------------------------

# kubectl get nodes

``` bash
kubectl get nodes
```

## Purpose

Displays every Node in the Kubernetes cluster.

Example:

``` text
NAME
control-plane
worker-1
worker-2
```

To view Node labels:

``` bash
kubectl get nodes --show-labels
```

## When to Use

Use this command to verify that the Nodes required by your workload
actually exist and have the expected labels.

------------------------------------------------------------------------

# How the Deployment Manifest Relates

Your Deployment manifest describes the **desired state** of the
workload. Kubernetes continuously compares that desired state with the
actual state of the cluster.

## Deployment Name

``` yaml
metadata:
  name: web
```

Appears as:

``` bash
kubectl get deployments
```

``` text
NAME
web
```

------------------------------------------------------------------------

## Replica Count

``` yaml
spec:
  replicas: 3
```

Appears as:

``` text
READY
3/3
```

If Kubernetes cannot create or schedule Pods, this may appear as:

``` text
0/3
```

------------------------------------------------------------------------

## Labels

``` yaml
labels:
  app: web
```

Verify with:

``` bash
kubectl get pods --show-labels
```

Labels allow Deployments, Services, and other resources to identify the
correct Pods.

------------------------------------------------------------------------

## Container Image

``` yaml
containers:
  - image: nginx:stable
```

Verify with:

``` bash
kubectl describe pod <pod-name>
```

Incorrect image names often result in an **ImagePullBackOff** status.

------------------------------------------------------------------------

## nodeSelector

``` yaml
nodeSelector:
  kubernetes.io/hostname: worker-1
```

This instructs Kubernetes to schedule the Pod only on Nodes matching the
specified label.

If no matching Node exists, the Pod remains **Pending** until the
selector is corrected or an appropriate Node becomes available.

------------------------------------------------------------------------

# Common Scheduling Problems

  -----------------------------------------------------------------------
  Issue               Possible Cause
  ------------------- ---------------------------------------------------
  Pods remain Pending Invalid nodeSelector, node affinity, taints, or
                      insufficient resources

  Deployment shows    Pods failed to schedule or start
  0/X READY           

  ImagePullBackOff    Invalid image name or registry access issue

  CrashLoopBackOff    Application starts but repeatedly crashes
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# Helpful kubectl Commands

``` bash
kubectl get deployments
kubectl get pods
kubectl describe pod <pod-name>
kubectl get nodes
kubectl get nodes --show-labels
kubectl get pods --show-labels
kubectl rollout status deployment/<deployment-name>
kubectl logs <pod-name>
```

------------------------------------------------------------------------

# Key Takeaways

-   Begin troubleshooting with `kubectl get deployments`.
-   Use `kubectl get pods` to identify affected workloads.
-   Use `kubectl describe pod` to inspect scheduler events and
    configuration.
-   Compare Pod requirements in the YAML manifest with the available
    cluster resources.
-   Correct the manifest and reapply it with `kubectl apply`.
-   The Deployment manifest defines the desired state; `kubectl`
    commands help verify the current state.

------------------------------------------------------------------------

# Official Documentation

-   Kubernetes Documentation: https://kubernetes.io/docs/
-   kubectl Cheat Sheet:
    https://kubernetes.io/docs/reference/kubectl/cheatsheet/
-   Troubleshooting Applications:
    https://kubernetes.io/docs/tasks/debug/debug-application/
