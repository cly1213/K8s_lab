## Certificates
PKI Certificates

locations 

```
ls -l /etc/kubernetes/

admin.conf

kubectl.conf

scheduler.conf

controller-manager.conf

```
## Kubelet

## Kube-proxy
part of network function

Type : Service(ClusterIP/NodePort..etc)


Proxy mode:

Userspace(slow)/iptables(default)/ipvs(kernel)
 - required different kernel modules


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




