# CNI
- Pod to Pod
- CNI choose
- CNI debug
- NetworkPolicy (like as firewall)

```
kubectl -n kube-system get ds

kubectl -n kube-system serviceaccount kindnet

kubectl -n kube-system get clusterrole kindnet

kubectl -n kube-system get clusterrolebinding kindnet

docker exec -it kind-worker bash

ls /etc/cni/net.d/

ls /var/lib/cni/results

docker exec -it kind-control-plane bash

ps auxw | grep ...
```

## Service/Ingress
 - ClusterIP
 - NodePort
 - Load-Balancer
 - Headless

 ## Ingress
 HTTP/HTTPS based routing

 port 80: http
 port 443: https

 ## DNS
 