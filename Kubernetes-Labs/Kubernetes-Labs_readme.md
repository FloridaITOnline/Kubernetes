# Kubernetes Labs

> My personal Kubernetes learning repository documenting my journey from
> Kubernetes fundamentals through hands-on administration.

------------------------------------------------------------------------

## About

This repository contains my notes, labs, YAML manifests, and
walkthroughs as I work through the **Kubernetes and Cloud Native
Associate (KCNA)** certification and continue building practical
Kubernetes experience.

Rather than simply collecting commands, the goal of this repository is
to understand **how Kubernetes works**, document best practices, and
build a reference I can use in future projects.

The labs are based on modern Kubernetes releases and updated
installation methods where appropriate.

------------------------------------------------------------------------

## Goals

-   Learn Kubernetes fundamentals
-   Build clusters using `kubeadm`
-   Understand Kubernetes architecture
-   Deploy and manage workloads
-   Learn networking and storage
-   Practice troubleshooting
-   Build a portfolio of Kubernetes examples

------------------------------------------------------------------------

## Repository Structure

``` text
.
├── README.md
├── 01-Building-a-Kubernetes-Cluster-with-kubeadm-v1.35.md
├── 02-Kubernetes-Architecture.md
├── 03-Pods.md
├── 04-ReplicaSets.md
├── 05-Deployments.md
├── 06-Services.md
├── 07-Ingress.md
├── 08-StatefulSets.md
├── 09-ConfigMaps-and-Secrets.md
├── 10-Persistent-Volumes.md
├── 11-Namespaces.md
├── 12-Helm.md
├── 13-Troubleshooting.md
└── labs/
```

------------------------------------------------------------------------

## Topics Covered

-   Kubernetes Architecture
-   kubeadm
-   kubectl
-   containerd
-   Pods
-   ReplicaSets
-   Deployments
-   StatefulSets
-   DaemonSets
-   Jobs & CronJobs
-   Services
-   Ingress
-   ConfigMaps
-   Secrets
-   Persistent Volumes
-   Persistent Volume Claims
-   Namespaces
-   Networking
-   Calico CNI
-   Scheduling
-   Helm
-   Cluster Troubleshooting

------------------------------------------------------------------------

## Lab Environment

-   Ubuntu 20.04
-   Kubernetes 1.35
-   containerd
-   Calico CNI
-   Three-node cluster
    -   1 Control Plane
    -   2 Worker Nodes

------------------------------------------------------------------------

## Philosophy

This repository is intended to document **understanding**, not just
completed exercises.

Where possible I: - Explain why commands are used. - Update older labs
to current Kubernetes practices. - Include working YAML examples. -
Document troubleshooting steps and lessons learned.

------------------------------------------------------------------------

## Future Plans

-   Deploy a multi-tier application
-   Learn Helm packaging
-   Explore GitOps
-   Deploy monitoring with Prometheus and Grafana
-   Explore Kubernetes security
-   Continue toward the Certified Kubernetes Administrator (CKA)

------------------------------------------------------------------------

## References

-   Kubernetes Documentation
-   CNCF KCNA Learning Path
-   kubeadm Documentation
-   Calico Documentation

------------------------------------------------------------------------

## License

This repository is provided for educational purposes.
