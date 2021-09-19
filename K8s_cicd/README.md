#  CI/CD Workflow

Makefile 
```
sudo make build-image
sudo make REPO=name/repo build-image

sudo docker login

sudo make push-image
sudo make REPO=name/repo push-image

sudo make VERSION=release build-image
```

```
$ kubectl apply -f yamls/
configmap/index2 created
deployment.apps/debug-pod created
deployment.apps/nginx created
service/nginx created

$ kubectl get svc
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1     <none>        443/TCP        6h10m
nginx        NodePort    10.96.3.255   <none>        80:31409/TCP   6s

$ kubectl get nodes -o=wide
NAME                 STATUS   ROLES    AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE       KERNEL-VERSION      CONTAINER-RUNTIME
kind-control-plane   Ready    master   6h11m   v1.17.0   172.18.0.2    <none>        Ubuntu 19.10   4.15.0-72-generic   containerd://1.3.2
kind-worker          Ready    <none>   6h11m   v1.17.0   172.18.0.4    <none>        Ubuntu 19.10   4.15.0-72-generic   containerd://1.3.2
kind-worker2         Ready    <none>   6h11m   v1.17.0   172.18.0.3    <none>        Ubuntu 19.10   4.15.0-72-generic   containerd://1.3.2

# 每個docker都是一個node
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED       STATUS       PORTS                       NAMES
e00d2e21f301   kindest/node:v1.17.0   "/usr/local/bin/entr…"   6 hours ago   Up 6 hours                               kind-worker
295738a9bb63   kindest/node:v1.17.0   "/usr/local/bin/entr…"   6 hours ago   Up 6 hours   127.0.0.1:32768->6443/tcp   kind-control-plane
6e71cbc4e188   kindest/node:v1.17.0   "/usr/local/bin/entr…"   6 hours ago   Up 6 hours

$ kubectl get pods -o=wide
NAME                         READY   STATUS    RESTARTS   AGE   IP           NODE           NOMINATED NODE   READINESS GATES
debug-pod-6845ffcd87-zmrfl   1/1     Running   0          14m   10.244.2.2   kind-worker2   <none>           <none>
nginx-7ff4d8766-ncfvb        1/1     Running   0          14m   10.244.2.3   kind-worker2   <none>           <none>
nginx-7ff4d8766-xcr6d        1/1     Running   0          14m   10.244.1.3   kind-worker    <none>           <none>
nginx-7ff4d8766-xjjqg        1/1     Running   0          14m   10.244.1.2   kind-worker    <none>           <none>                               kind-worker2
```

```
curl 172.18.0.3:31409
Hostname is ch1_nginx-7ff4d8766-xcr6d
Latest Commit Hash is ch1_2c3e248
Latest Commit Log is ch1_update new password
IP address list are ch1_10.244.1.3
Datetime is Thu Sep 16 10:58:43 UTC 2021

curl 172.18.0.4:31409
Hostname is ch1_nginx-7ff4d8766-xjjqg
Latest Commit Hash is ch1_2c3e248
Latest Commit Log is ch1_update new password
IP address list are ch1_10.244.1.2
Datetime is Thu Sep 16 10:58:42 UTC 2021
```