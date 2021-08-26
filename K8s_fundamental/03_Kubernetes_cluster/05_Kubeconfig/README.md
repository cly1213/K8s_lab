## Kubeconfig
https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/

```
vim ~/.kube/config 

KUBECONFIG=~/.kube/config kubectl config get-contexts
KUBECONFIG=~/.kube/config kubectl -n kube-system get pods
kubectl --kubeconfig=~/.kube/config config get-contexts

# Use rancher
kubectl --kubeconfig rancher config get-contexts

# Environmental variables
KUBECONFIG=rancher kubectl config get-contexts

KUBECONFIG=rancher kubectl -n kube-system get pods

```

```
kubectl config get-contexts
kubectl config use-context $NAME
```

Merge Kubeconfig

```
cp ~/.kube/config ~/.kube/config_bk

# merge 2 file => file1:file2:file3:...
KUBECONFIG=rancher:~/.kube/config kubectl config view --flatten > new

KUBECONFIG=new kubectl config get-contexts 

KUBECONFIG=new kubectl config use-contexts $NAME 
```
