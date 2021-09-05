## Certificates
PKI Certificates

```
## locations  
ls -l /etc/kubernetes/

admin.conf

kubectl.conf

scheduler.conf

controller-manager.conf
```
## Kubelet

```
docker ps

docker exec -it kind-worker2 bash

ps auxw | grep containerd

ctr -n k8s.io image ls

ctr -n k8s.io c ls

crictl images

crictl ls

crictl pods

ps auxw | grep kubelet

systemctl stop containerd.service 

journalctl -xe

kubectl describe nodes kind-worker2

sudo kubeadm init --pod-network-cidr=10.244.0.0/16

## Turn off swap
sudo swapoff -a && sudo sysctl -w vm.swappiness=0

sudo kubeadm init --pod-network-cidr=10.244.0.0/16

kubectl -n kube-system get pods

sudo reboot

kubectl get pods

systemctl status kubelet

journalctl -xe

## Restart turn off swap
sudo swapoff -a && sudo sysctl -w vm.swappiness=0
```

## Kube-proxy
part of network function

Type : Service(ClusterIP/NodePort..etc)


Proxy mode:

Userspace(slow)/iptables(default)/ipvs(kernel)
 - required different kernel modules

See iptables

```
iptables-save

iptables-save | wc
```

### iptables
Tools
  - iptables
  - iptables-save

Require kernel module
  - statisitc, tcpdump, conntrack..etc

### ipvs

LoadBalancer

Tools
  - ipvsadm

Require kernel module
  - ip_vs
  - ip_vs_rr
  - ip_vs_wrr
  - ip_vs_sh

```
sudo kubeadm init --config kubeconfig.yaml

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml

kubectl taint nodes k8s-dev node-role.kubernetes.io/master:NoSchedule-

kubectl apply -f service.yml

sudo ipvsadm -ln

sudo ipvsadm -ln | wc
```

## Nodes

```
kubectl get nodes

kubectl describe nodes
```

Node

Initialize kubeadm

```
sudo kubeadm init  --apiserver-advertise-address=172.17.8.123 --pod-network-cidr=10.244.0.0/16

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml

kubectl taint nodes host1 node-role.kubernetes.io/master:NoSchedule-
```

Join node (2ed VM)

```
sudo hostnamectl set-hostname host1

kubectl describe node host1

sudo kubeadm reset
```

Pod Max

```
kubectl apply -f service.yaml

kubectl describe pods ....
```

Disk full

```
dd if=/dev/zero of=a bs=8k

df -h

systemctl restart kubelet
```



