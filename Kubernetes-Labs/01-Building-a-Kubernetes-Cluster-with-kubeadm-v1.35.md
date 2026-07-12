# Building a Kubernetes Cluster with kubeadm (Kubernetes 1.35)

## Overview

This guide walks through building a three-node Kubernetes cluster using
**kubeadm**, **containerd**, and **Ubuntu 20.04**.

Unlike many older tutorials, this guide uses the modern Kubernetes
package repository (`pkgs.k8s.io`) instead of the deprecated
`apt.kubernetes.io` repository.

## Lab Topology

  Host             Role
  ---------------- ---------------
  k8s-controller   Control Plane
  k8s-worker1      Worker Node
  k8s-worker2      Worker Node

------------------------------------------------------------------------

# Step 1 -- Configure Hostnames

## Controller

``` bash
sudo hostnamectl set-hostname k8s-controller
```

## Worker 1

``` bash
sudo hostnamectl set-hostname k8s-worker1
```

## Worker 2

``` bash
sudo hostnamectl set-hostname k8s-worker2
```

Log out and back in after changing the hostname.

------------------------------------------------------------------------

# Step 2 -- Configure `/etc/hosts`

On **all three machines**, add:

``` text
<CONTROLLER_IP> k8s-controller
<WORKER1_IP>    k8s-worker1
<WORKER2_IP>    k8s-worker2
```

Verify:

``` bash
getent hosts k8s-controller
getent hosts k8s-worker1
getent hosts k8s-worker2
```

------------------------------------------------------------------------

# Step 3 -- Enable Required Kernel Modules

``` bash
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

Verify:

``` bash
lsmod | grep overlay
lsmod | grep br_netfilter
```

------------------------------------------------------------------------

# Step 4 -- Configure Kubernetes Networking

``` bash
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system
```

Verify:

``` bash
sysctl net.ipv4.ip_forward
```

Expected:

``` text
net.ipv4.ip_forward = 1
```

------------------------------------------------------------------------

# Step 5 -- Install containerd

``` bash
sudo apt update
sudo apt install -y containerd

sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml

sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' \
/etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl enable containerd
```

Verify:

``` bash
sudo systemctl status containerd
```

------------------------------------------------------------------------

# Step 6 -- Disable Swap

``` bash
sudo swapoff -a
```

Comment out any swap entries in:

``` text
/etc/fstab
```

Verify:

``` bash
swapon --show
```

No output should be returned.

------------------------------------------------------------------------

# Step 7 -- Install Kubernetes 1.35

Remove any legacy repository:

``` bash
sudo rm -f /etc/apt/sources.list.d/kubernetes.list
```

Install prerequisites:

``` bash
sudo apt update
sudo apt install -y ca-certificates curl gpg
```

Create the keyring:

``` bash
sudo mkdir -p -m 755 /etc/apt/keyrings
```

Import the signing key:

``` bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.35/deb/Release.key \
| sudo gpg --dearmor \
-o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Add the repository:

``` bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.35/deb/ /' \
| sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Install Kubernetes:

``` bash
sudo apt update

sudo apt install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl
```

Verify:

``` bash
kubeadm version -o short
kubectl version --client
kubelet --version
```

------------------------------------------------------------------------

# Step 8 -- Initialize the Control Plane

Run **only** on the controller.

``` bash
sudo kubeadm init \
  --pod-network-cidr=192.168.0.0/16
```

Configure kubectl:

``` bash
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

------------------------------------------------------------------------

# Step 9 -- Install Calico

``` bash
kubectl apply -f \
https://raw.githubusercontent.com/projectcalico/calico/master/manifests/calico.yaml
```

Verify:

``` bash
kubectl get pods -n kube-system
```

------------------------------------------------------------------------

# Step 10 -- Join Worker Nodes

Generate the join command on the controller:

``` bash
kubeadm token create --print-join-command
```

Run the generated command on **both worker nodes**.

Example:

``` bash
sudo kubeadm join <CONTROLLER_IP>:6443 \
  --token <TOKEN> \
  --discovery-token-ca-cert-hash sha256:<HASH>
```

------------------------------------------------------------------------

# Step 11 -- Verify the Cluster

``` bash
kubectl get nodes -o wide
```

Expected:

``` text
NAME             STATUS   ROLES           VERSION
k8s-controller   Ready    control-plane   v1.35.x
k8s-worker1      Ready    <none>          v1.35.x
k8s-worker2      Ready    <none>          v1.35.x
```

------------------------------------------------------------------------

# What I Learned

-   Built a Kubernetes cluster with kubeadm.
-   Configured containerd as the CRI runtime.
-   Enabled required Linux kernel modules.
-   Configured pod networking prerequisites.
-   Installed Calico CNI.
-   Joined worker nodes to the control plane.
-   Verified cluster health using kubectl.

------------------------------------------------------------------------

# Notes

This guide intentionally uses the **modern Kubernetes 1.35 installation
method** (`pkgs.k8s.io` and keyrings) instead of the deprecated
`apt.kubernetes.io` repository used by many older tutorials.
