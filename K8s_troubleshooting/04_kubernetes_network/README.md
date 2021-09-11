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

```
kubectl get endpoints python -o json | jq '.subsets[0].addresses[].ip'

docker exec kind-worker iptables -t filter -D FORWARD -i eth0 -p tcp --dport 8000  -j DROP

curl 172.19.0.2:xxxxx/hostname

```


## DNS

```
kubectl -n kube-system edit cm coredns

kubectl -n kube-system logs -f -l k8s-app=kube-dns

cat /etc/resolv.conf

tcpdump -vvvvn -i eth0 port 53

nslookup google.com

nslookup google.com.
```

## Debug