# Kubernetes

Welcome to the Kubernetes section of my technology repository.

This directory contains everything related to my Kubernetes
journey---from foundational labs completed while studying for the
**Kubernetes and Cloud Native Associate (KCNA)** certification to
reusable configurations, deployment manifests, scripts, troubleshooting
notes, and reference material.

The goal is to build a practical knowledge base that I can reference in
future projects while also documenting the concepts and techniques I
learn along the way.

------------------------------------------------------------------------

# Objectives

-   Develop a solid understanding of Kubernetes fundamentals.
-   Build and administer Kubernetes clusters.
-   Create reusable manifests and configuration examples.
-   Document common administration tasks and troubleshooting techniques.
-   Maintain a personal reference library for future Kubernetes
    projects.
-   Continue progressing toward more advanced Kubernetes certifications
    such as the **Certified Kubernetes Administrator (CKA)**.

------------------------------------------------------------------------

# Directory Structure

``` text
Kubernetes/
├── README.md
├── labs/
│   ├── README.md
│   ├── lab01/
│   ├── lab02/
│   └── ...
│
├── manifests/
│   ├── deployments/
│   ├── services/
│   ├── ingress/
│   ├── storage/
│   └── ...
│
├── configs/
│   ├── containerd/
│   ├── kubeadm/
│   ├── calico/
│   └── ...
│
├── scripts/
│
├── troubleshooting/
│
├── notes/
│
└── images/
```

------------------------------------------------------------------------

# Contents

## Labs

Hands-on exercises used to reinforce Kubernetes concepts through
practical implementation.

Examples include:

-   Cluster installation with kubeadm
-   Pods
-   ReplicaSets
-   Deployments
-   Services
-   Ingress
-   StatefulSets
-   Persistent Storage
-   Helm
-   Troubleshooting

------------------------------------------------------------------------

## Manifests

Reusable Kubernetes YAML examples for common resources.

Examples include:

-   Pods
-   Deployments
-   Services
-   Ingress
-   ConfigMaps
-   Secrets
-   Persistent Volumes
-   Persistent Volume Claims

------------------------------------------------------------------------

## Configurations

Configuration files for commonly used Kubernetes components and
supporting software.

Examples include:

-   kubeadm
-   containerd
-   Calico
-   kubelet
-   networking
-   system configuration

------------------------------------------------------------------------

## Scripts

Helper scripts used for cluster administration, automation, and
repeatable deployment tasks.

------------------------------------------------------------------------

## Troubleshooting

Real-world issues encountered while building or administering Kubernetes
clusters along with their resolutions.

Examples include:

-   Node join failures
-   Networking issues
-   Container runtime problems
-   kubelet troubleshooting
-   Certificate issues
-   Repository migration (`apt.kubernetes.io` → `pkgs.k8s.io`)

------------------------------------------------------------------------

## Notes

General reference material, architecture diagrams, command summaries,
and study notes collected throughout the learning process.

------------------------------------------------------------------------

# Environment

Current lab environment:

-   Ubuntu 20.04
-   Kubernetes 1.35
-   containerd
-   Calico CNI
-   kubeadm
-   Three-node cluster
    -   1 Control Plane
    -   2 Worker Nodes

------------------------------------------------------------------------

# Repository Philosophy

This repository is intended to be more than a collection of lab
exercises.

Whenever possible I try to:

-   Explain why a configuration or command is used.
-   Update older tutorials to reflect current Kubernetes best practices.
-   Document lessons learned and troubleshooting steps.
-   Create reusable examples for future projects.
-   Build a reference library that continues to grow beyond KCNA.

As my Kubernetes experience grows, this directory will expand with
additional projects, automation, and production-oriented examples.
