## Network Functions
https://kubernetes.io/docs/concepts/services-networking/

Network Connectivity
  - Conditions
    - Pod to Pod
    - Wan to Pod
    - Pod to Wan
  - Protocols
    - Layer 4: TCP/UDP/STCP
    - Layer 7: HTTP/gRPC

### Flannel & CNI

https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

https://github.com/GoogleCloudPlatform/container-engine-accelerators/blob/master/daemonset.yaml

- DaemonSet -> each node

### IPAM (IP Management)
Pod to Pod
  - In same Node
    - Simple

  - Cross Nodes
    - Complex(VXLAN by default)

## Service
https://kubernetes.io/docs/concepts/services-networking/service/

Provide Virtual IP(VIP)


### Kubernete Service
Application to Service
  - Use DNS to access the service

Service Pods
  - Controller maintains a IP:Port list for service

Implementation
  - Userspace
  - Iptables(default)
  - IPVS

Kube-proxy
  - --proxy-mode

Types
  - ClusterIP
  - NodePort
  - LoadBalancer
  - ExternalName
  - Headless

#### ClusterIP
  - client A: Outside the kubernetes node(VM)
  - client B: Inside the kubernetes node

https://github.com/paulbouwer/hello-kubernetes

```
kubectl apply -f hello.yml
kubectl apply -f service.yml

or
kubectl apply -R -f clusterIP/

kubectl get pods -o wide

kubectl api-resource

kubectl get svc
kubectl get ep

clusterIP=$(kubectl get svc cluster-demo -o jsonpath='{.spec.clusterIP}')

# from host
curl $clusterIP

# from node
docker exec kind-worker $clusterIP

# from client-pod
client_pod=$(kubectl get pods -l app=client -o jsonpath='{.items[0].metadata.name}')
kubectl exec ${client_pod} curl ${clusterIP}
kubectl exec ${client_pod} curl cluster-demo


kubectl delete -R -f clusterIP/
```

#### NodePort
Exposes the Service on each Node's IP at static port(the NodePort)

Be able to contact NodePort Service, from outside the cluster

```
kindIP=$(docker inspect kind-worker  | jq '.[0].NetworkSettings.Networks.bridge.IPAddress' | tr -d '""')

echo $kindIP
172.18.0.3

NODEPORT=$(kubectl get svc nodeport-demo  -o jsonpath='{.spec.ports[0].nodePort}')

echo $NODEPORT
32282

# Add rules
sudo iptables -t filter -I DOCKER -d $kindIP/32 ! -i docker0 -o docker0 -p tcp -m tcp --dport $NODEPORT -j ACCEPT

sudo iptables -t nat -I DOCKER -d 172.17.8.111/32 ! -i docker0 -p tcp -m tcp --dport 8080 -j DNAT --to-destination $kindIP:$NODEPORT

# Open brower 172.17.8.111:8080

# remove rules
sudo iptables -t filter -D DOCKER -d $kindIP/32 ! -i docker0 -o docker0 -p tcp -m tcp --dport $NODEPORT -j ACCEPT


sudo iptables -t nat -D DOCKER -d 172.17.8.111/32 ! -i docker0 -p tcp -m tcp --dport 8080 -j DNAT --to-destination $kindIP:$NODEPORT
```

<img src="https://github.com/cly1213/K8s_labs/blob/main/image/NodePort.png"/>


#### Headless
Do the load-balancing by yourself

```
kubectl apply -R -f headless

kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
client-67674d5464-zcs8h   1/1     Running   0          36s
hello-kubernetes-0        1/1     Running   0          36s
hello-kubernetes-1        1/1     Running   0          34s
hello-kubernetes-2        1/1     Running   0          32s

kubectl get svc
NAME            TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
headless-demo   ClusterIP   None         <none>        80/TCP    5s
kubernetes      ClusterIP   10.96.0.1    <none>        443/TCP   16m

kubectl get ep
NAME            ENDPOINTS                                         AGE
headless-demo   10.244.1.5:8080,10.244.2.4:8080,10.244.2.5:8080   14s
kubernetes      172.18.0.4:6443

kubectl get pods -o wide
NAME                      READY   STATUS    RESTARTS   AGE     IP           NODE           NOMINATED NODE   READINESS GATES
client-67674d5464-zcs8h   1/1     Running   0          5m39s   10.244.1.4   kind-worker    <none>           <none>
hello-kubernetes-0        1/1     Running   0          5m39s   10.244.2.4   kind-worker2   <none>           <none>
hello-kubernetes-1        1/1     Running   0          5m37s   10.244.1.5   kind-worker    <none>           <none>
hello-kubernetes-2        1/1     Running   0          5m35s   10.244.2.5   kind-worker2   <none>           <none>


kubectl exec client-67674d5464-zcs8h nslookup hello-kubernetes-0.headless-demo
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Server:		10.96.0.10
Address:	10.96.0.10#53

Name:	hello-kubernetes-0.headless-demo.default.svc.cluster.local
Address: 10.244.2.4

kubectl exec client-67674d5464-zcs8h nslookup hello-kubernetes-1.headless-demo
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Server:		10.96.0.10
Address:	10.96.0.10#53

Name:	hello-kubernetes-1.headless-demo.default.svc.cluster.local
Address: 10.244.1.5

kubectl exec client-67674d5464-zcs8h nslookup hello-kubernetes-2.headless-demo
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Server:		10.96.0.10
Address:	10.96.0.10#53

Name:	hello-kubernetes-2.headless-demo.default.svc.cluster.local
Address: 10.244.2.5
```

#### Load Balancer/ExternalName

#### Ingress
