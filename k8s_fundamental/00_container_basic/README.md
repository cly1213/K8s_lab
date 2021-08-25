# Container
From docker

## Rersistrnt volume
- $ mount (-v)

## Network
- container to WAN
- WAN to container
- container to container

### Docker Network Linux Bridge

Docker utilizing the following mechanism
- Linux Bridge
	1. Spanning Tree
	2. Forwarding Table
	3. Layer 2 forwarding

- Routing Table
	1. Layer 3 routing

- ARP Table

- Netfilter system
	1. iptables

- Veth(Virtual Ethernet)

- Network Namespace

## Deployment
- Single Container/Single Node
- Multiple Container/Single Node
	1. Docker-compose(support DNS for container network access)

- Multiple Container/Multiple Node
	1. Docker-swarm
	2. Kubernetes

## Reference

LXC v.s Docker

https://www.upguard.com/articles/docker-vs-lxc

PodMan v.s Docker

https://developers.redhat.com/blog/2019/02/21/podman-and-buildah-for-docker-users/

Kata Container v.s Docker

https://superuser.openstack.org/articles/how-kata-containers-boost-security-in-docker-containers/

CRI-O v.s Docker

https://thenewstack.io/cri-o-project-run-containers-without-docker-reaches-1-0/

Online learning

https://katacoda.com
