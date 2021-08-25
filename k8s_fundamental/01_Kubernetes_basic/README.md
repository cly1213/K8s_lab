# K8s
## CNCF (Cloud Native)
https://landscape.cncf.io/

## 
### Control Plane/Master
- API server

- Etcd
    - key/value store
    - store for all cluster data

- Scheduler

- Controller

### Agent
#### Node
- Kubelet
- Kube-proxy
- Container-runtime
    - CRI Implementation

## K8s(Kubernetes) & CRI(Container Runtime Interface)

## K8s - CNI(Container Network Interface)
CNI Implementation
- Choose a CNI for your kubernetes cluster

## K8s - CSI(Container Storage Interface)
CRI Implementation
- was developed as a standard for exposing block/fs to containerized workloads.
- Choose the storage for your cluster based on your requirements.

## Reference
Kubernetes History

https://kubernetes.io/blog/2018/07/20/the-history-of-kubernetes-the-community-behind-it/

Borg:

https://kubernetes.io/blog/2015/04/borg-predecessor-to-kubernetes/

Borg/Omeg/Kuberentes:

https://dl.acm.org/doi/pdf/10.1145/2898442.2898444?download=true

https://cloud.google.com/blog/products/gcp/from-google-to-the-world-the-kubernetes-origin-story

Borg paper:

https://storage.googleapis.com/pub-tools-public-publication-data/pdf/43438.pdf

CNCF:

https://www.cncf.io/

https://www.cncf.io/cncf-kubernetes-project-journey/

https://docs.google.com/presentation/d/1BoxFeENJcINgHbKfygXpXROchiRO2LBT-pzdaOFr4Zg/edit#slide=id.g5787306e70_0_0

CRI:

https://github.com/containerd/cri

CNI:

https://github.com/containernetworking/cni

https://github.com/containernetworking/plugins/tree/master/plugins/main/bridge

CSI:

https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/