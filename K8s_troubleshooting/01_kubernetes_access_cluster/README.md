# Cluster access

## config (kubeconfig) <---(1)---> kubectl <---(2)/(3)---> API server

Clusters/Users/Contexts

- Context: Who(User), Where(Cluster)

kubectl or helm

```
KUBECONFIG=/tmp kubectl get nodes
kubectl --kubeconfig config get nodes

kubectl config view
kubectl config get-contexts
vim ~/.kube/config
```

## RBAC
Role Based Access Control

- Subject(Who)
  - Developer/Process/Pod

- Resources
  - Pod/Deployment/PV/DaemonSet

- Verbs
  - list/get/create/watch/patch

Role

RoleBinding

## Kubectl
```

```


## K9s
https://k9scli.io/


