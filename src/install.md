# Installation Guide

Welcome to the AppNet installation guide. This document provides step-by-step instructions on how to set up AppNet and its dependencies.

> **Note:** The following scripts are tested on Ubuntu 20.04. We plan to add support for other platforms soon.

### Cloning the Repository

First, clone the AppNet repo:
```bash
git clone git@github.com:appnet-org/appnet.git --recursive
cd appnet
```

Before you start, make sure you have the following installed:

Requirements:
 - [Kubernetes](#kubernetes) (v1.28+) 
 - [Istio](#istio) (v1.22.x) 
    - Note: AppNet's support for istio 1.23.x is in progress.
 - [Go](#go) (v1.22.2)
 - [Rust](#rust) (v1.71+)
 - [protoc](#protoc)
 - [Conda](#conda)


### Install the CLI

`appnetctl` is a command line program to manage the AppNet control plane. To install the CLI, follow these steps:

1. Create a Conda environment:
```bash
conda create -y -n appnet python=3.10
conda activate appnet
```

2. Run the installation script:
```bash
. ./install.sh
```

3. Verify the CLI and other tools are running correctly:
```bash
user@h1:~/appnet$ appnetctl version
Version: v0.1.0
user@h1:~/appnet$ appnetctl verify
Verifying AppNet installation status...
✔ Python installed.
✔ Rust installed.
✔ Kubernetes installed.
✔ protoc installed.
✔ Istio installed.
```

4. Lastly, install the CRDs into the cluster:

```sh
make install
```


## Requirements

### Kubernetes
To install a Kubernetes cluster, we recommend using kubeadm. Follow the steps below:

1. Install the Control Plane:
```bash
. ./utils/k8s_setup.sh
```

2. (Optional) Set Up Worker Nodes:
 - First, prepare the worker nodes:
 ```bash
 . ./utils/k8s_setup_worker.sh
 ```

 - Then, join the cluster using kubeadm join. Run the following command on the control plane node to get the join command:
 ```bash
 kubeadm token create --print-join-command
 ```

Note: if you plan to use a multi-node cluster, make sure you can ssh other nodes from the control plane node.

3. Verify Installation:
```bash
kubectl version
```

For additional installation options (e.g., KIND, Minikube), visit this [page](https://kubernetes.io/docs/tasks/tools/)

(Optional) We highly recommend installing [k9s](https://k9scli.io/topics/install/) for visualizing your clutser.

### Istio

Istio can be installed in either sidecar mode or ambient mode. Choose the one that best fits your requirements (if you want to use both sidecar and ambient mode, please install using ambient script):

- Sidecar Mode
```bash
. ./utils/istio_setup_sidecar.sh
```

- Ambient Mode
```bash
. ./utils/istio_setup_ambient.sh
```

### Go

Install Go by running the following command:
```
wget https://go.dev/dl/go1.22.1.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.1.linux-amd64.tar.gz

echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
source ~/.bashrc
```

### Rust
Install Rust by running the following command:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### Protoc
Install the Protocol Buffers Compiler and the necessary Go plugins with these commands:
```bash
sudo apt -y install protobuf-compiler
```

### Conda

See this [page](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) for installation instructions.

For Ubuntu users:
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py310_23.3.1-0-Linux-x86_64.sh -O Miniconda.sh
bash Miniconda.sh
```
