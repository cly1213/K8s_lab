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
Use Kubeadm

[note] frequently used


```
apply
 - kubectl apply -f ...(file)
 - kubectl apply -f .../(directory)

diff

get -f ... -o yaml 
```

### Pause Container
Create first and be attached by other containers

Network core 

```
docker run -d --name test netutils
docekr run -d --net=container:test --name test2 netutils

docker ps | grep test
docker exec -it test ifconfig
docker exec -it test2 ifconfig
docker rm -f test

docker run -d --net=host --name test3 netutils


kubectl apply -f pause.yaml

kubectl get pods

# select container
kubectl exec -it two-container -c www-server bash

```

### ReplicaSet
Use KIND

[note] rarely used

To replicate more Pods

```
$ kubectl apply -f basic.yaml
replicaset.apps/test-rs created

$ cat basic.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: test-rs
  labels:
    app: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-server
        image: nginx

```

### Kubectl Plugin - Tree
- https://krew.dev/

- https://github.com/kubernetes-sigs/krew-index/blob/master/plugins.md

Install krew

```
set -x; cd "$(mktemp -d)" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/download/v0.3.4/krew.{tar.gz,yaml}" &&
  tar zxvf krew.tar.gz &&
  KREW=./krew-"$(uname | tr '[:upper:]' '[:lower:]')_amd64" &&
  "$KREW" install --manifest=krew.yaml --archive=krew.tar.gz &&
  "$KREW" update 

echo 'export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"' >> ~/.bashrc
```

```
$ kubectl krew install tree

$ kubectl tree rs test-rs
NAMESPACE  NAME                 READY  REASON  AGE
default    ReplicaSet/test-rs   -              28s
default    ├─Pod/test-rs-6j58z  True           28s
default    ├─Pod/test-rs-fh5c2  True           28s
default    └─Pod/test-rs-tm4q2  True           28s

$ kubectl tree -n kube-system deployment coredns
NAMESPACE    NAME                              READY  REASON  AGE
kube-system  Deployment/coredns                -              2m38s
kube-system  └─ReplicaSet/coredns-6955765f44   -              2m22s
kube-system    ├─Pod/coredns-6955765f44-jsnqd  True           2m22s
kube-system    └─Pod/coredns-6955765f44-l9mwg  True           2m22s

$ kubectl tree -n kube-system daemonset kube-proxy
NAMESPACE    NAME                                       READY  REASON  AGE
kube-system  DaemonSet/kube-proxy                       -              2m45s
kube-system  ├─ControllerRevision/kube-proxy-68bd87b66  -              2m30s
kube-system  ├─Pod/kube-proxy-6mcp8                     True           2m30s
kube-system  ├─Pod/kube-proxy-b4976                     True           2m8s
kube-system  └─Pod/kube-proxy-mrh2s                     True           2m8s

```

### Deployment

[note] frequently used(Recommand)

- Rollout Update
- Rollout Undo

```
kubectl rollout staths deployment test
kubectl rollout undo deployment test
```

### DaemonSet/StatefulSet

statefulset frequently used in  
  - network
  - storage

rolling update

```
kubectl tree -n kube-system ds kube-proxy
kubectl -n kube-system get ds
```

### Job/Conjob
https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/

conjob: time based

- mongodb (https://kubernetes.io/blog/2017/01/running-mongodb-on-kubernetes-with-statefulsets/)

- rollback daemonset (https://kubernetes.io/docs/tasks/manage-daemon/rollback-daemon-set/)

- ControllerRevision (https://github.com/kubernetes/api/blob/master/apps/v1/types.go#L791-L812)


 ## Debug
 - tools
 - log

    From Container
    - /var/logs
    - syslog
    - systemd

    From the system
    - dmesg
    - /var/log/
    - kern.log