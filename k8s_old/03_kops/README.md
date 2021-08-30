# Kops install Kubernetes cluster on Amazon AWS
## 1. Prepare Linux environment(or VM)

Install kops, kubectl, aws cli

```
$ kops version
$ kubectl version
$ aws --version
```

## 2. AWS
    - IAM (User & User group)
    - Route 53 (dns)
    - S3
    ...

```
# Create 
kops create cluster --name=k8s.lychen.link --state=s3://kops.k8s.lychen.link --zones=us-west-1a --node-count=2 --node-size=t2.medium --master-size=t2.medium --dns-zone=k8s.lychen.link

# Delete
kops delete cluster --name=k8s.lychen.link --state=s3://kops.k8s.lychen.link --yes

kops update cluster --name k8s.lychen.link --yes --admin --state=s3://kops.k8s.lychen.link
```

```
# Validate
$ kops validate cluster --state=s3://kops.k8s.lychen.link
Using cluster from kubectl context: k8s.lychen.link

Validating cluster k8s.lychen.link

INSTANCE GROUPS
NAME			ROLE	MACHINETYPE	MIN	MAX	SUBNETS
master-us-west-1a	Master	t2.medium	1	1	us-west-1a
nodes-us-west-1a	Node	t2.medium	2	2	us-west-1a

NODE STATUS
NAME						ROLE	READY
ip-172-20-32-156.us-west-1.compute.internal	master	True
ip-172-20-38-166.us-west-1.compute.internal	node	True
ip-172-20-61-31.us-west-1.compute.internal	node	True

Your cluster k8s.lychen.link is ready

$ kubectl get nodes
NAME                                          STATUS   ROLES                  AGE   VERSION
ip-172-20-32-156.us-west-1.compute.internal   Ready    control-plane,master   14m   v1.21.4
ip-172-20-38-166.us-west-1.compute.internal   Ready    node                   12m   v1.21.4
ip-172-20-61-31.us-west-1.compute.internal    Ready    node                   12m   v1.21.4
```


## 3. kops start

SSH key

```
ssh-keygen -f .ssh/id_rsa
```

