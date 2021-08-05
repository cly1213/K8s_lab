# minikube(linux)
## single node k8s cluster
[https://github.com/kubernetes/minikube](https://github.com/kubernetes/minikube)

## Install
Follow this guide

[https://kubernetes.io/docs/tasks/tools/install-minikube/](https://kubernetes.io/docs/tasks/tools/install-minikube/)

### Install a Hypervisor

We will choose VirtualBox

### Install kubectl

[https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin/kubectl
```

### Install Minikube

[https://github.com/kubernetes/minikube/releases](https://github.com/kubernetes/minikube/releases)

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version
```

## start
```bash
minikube start

```

## test
```bash
kubectl get nodes
```

## dashboard
```bash
$ minikube dashboard
```

## Delete minikube
```bash
minikube delete && rm -rf ~/.minikube
```
