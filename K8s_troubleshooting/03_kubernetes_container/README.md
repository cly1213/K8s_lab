# Container
## CRI
Container Runtime

<img src="https://github.com/cly1213/K8s_labs/blob/main/image/cri-o.png"/>

### Solutions
  - Kubeadm (default is dockerd, can customize)
  - KIND (default as containerd)
  - Kubernetes service
    - ASK (Docker/Containerd)
    - EKS (Docker/Containerd)
    - GKS (Docker/Containerd)


Install cri-o, podman, docker, kubectl

```
sudo kubeadm init --config kubeconfig.yaml

systemctl start cri-o

sudo crictl ps

ls /etc/cni/net.d

sudo netstat -anlpt | grep 12345

kubectl taint nodes k8s-dev node-role.kubernetes.io/master:NoSchedule-

ps auxw | grep 13560

sudo crictl pods

sudo systemctl start cri-o

sudo systemctl status cri-o

sudo systemctl status kubectl

## describe static pods info
ls /etc/kubernetes/manifests/

kubectl get pods
```

## Image
## Scheduling
  - Taint & Tolerations

  - NodeSelector

  - Drain
    - DeamonSet(Each node has)

## Pod
Phase
  - Pending
  - Running
  - Succeeded
  - Failed
  - Unknown



