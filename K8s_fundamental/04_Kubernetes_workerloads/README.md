#
## Pods
https://kubernetes.io/docs/concepts/workloads/pods/

- Status: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-and-container-status


Pod is the smallest workload unit in Kubernetes

Consist of container(s)

Share resources within the Pod
  - Networking
  - Storage
  - IPC


Remove Taint

```
kubectl get nodes -o jsonpath='{.items[0].spec.taints}'

kubectl taint nodes k8s-dev node-role.kubernetes.io/master:NoSchedule-
```

Ref:
- https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/

- https://kubernetes.io/docs/concepts/overview/working-with-objects/object-management/

- https://kubernetes.io/docs/reference/kubectl/cheatsheet/#editing-resources


### Kubectl Deployment Strategies - Imperative Object
- https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/


### Kubectl Deployment Strategies - Declarative Object

- https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/

- https://kubernetes.io/docs/concepts/overview/working-with-objects/object-management/#declarative-object-configuration

#### Declarative

apply
 - kubectl apply -f ...(file)
 - kubectl apply -f .../(directory)

diff

get -f ... -o yaml 





