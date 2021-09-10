# Eksctl create Kubernetes cluster on Amazon EKS
https://eksctl.io/

## Install eksctl
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

eksctl version
```

## cluster.yaml
```
$ cat cluster.yaml

apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: basic-cluster
  region: us-east-1

availabilityZones:
  - us-east-1a
  - us-east-1b

nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 2
    ssh:
      publicKeyPath: ~/.ssh/id_rsa.pub
    availabilityZones:
      - us-east-1a

  - name: ng-2
    instanceType: m5.xlarge
    desiredCapacity: 2
    ssh:
      publicKeyPath: ~/.ssh/id_rsa.pub
    availabilityZones:
      - us-east-1b

```


```
$ eksctl create cluster -f ./cluster.yaml

2021-09-06 08:34:42 [ℹ]  eksctl version 0.64.0
2021-09-06 08:34:42 [ℹ]  using region us-east-1
2021-09-06 08:34:42 [ℹ]  subnets for us-east-1a - public:192.168.0.0/19 private:192.168.64.0/19
2021-09-06 08:34:42 [ℹ]  subnets for us-east-1b - public:192.168.32.0/19 private:192.168.96.0/19
2021-09-06 08:34:43 [ℹ]  nodegroup "ng-1" will use "ami-01518178201f5d38f" [AmazonLinux2/1.20]
2021-09-06 08:34:44 [ℹ]  using SSH public key "/home/vagrant/.ssh/id_rsa.pub" as "eksctl-basic-cluster-nodegroup-ng-1-ce:61:e6:3a:04:c3:85:66:05:c3:cc:a2:d9:d2:00:4f"
2021-09-06 08:34:45 [ℹ]  using Kubernetes version 1.20
2021-09-06 08:34:45 [ℹ]  creating EKS cluster "basic-cluster" in "us-east-1" region with un-managed nodes
2021-09-06 08:34:45 [ℹ]  1 nodegroup (ng-1) was included (based on the include/exclude rules)
2021-09-06 08:34:45 [ℹ]  will create a CloudFormation stack for cluster itself and 1 nodegroup stack(s)
2021-09-06 08:34:45 [ℹ]  will create a CloudFormation stack for cluster itself and 0 managed nodegroup stack(s)
2021-09-06 08:34:45 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-1 --cluster=basic-cluster'
2021-09-06 08:34:45 [ℹ]  CloudWatch logging will not be enabled for cluster "basic-cluster" in "us-east-1"
2021-09-06 08:34:45 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-east-1 --cluster=basic-cluster'
2021-09-06 08:34:45 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "basic-cluster" in "us-east-1"
2021-09-06 08:34:45 [ℹ]  2 sequential tasks: { create cluster control plane "basic-cluster", 3 sequential sub-tasks: { wait for control plane to become ready, 1 task: { create addons }, create nodegroup "ng-1" } }
2021-09-06 08:34:45 [ℹ]  building cluster stack "eksctl-basic-cluster-cluster"
2021-09-06 08:34:47 [ℹ]  deploying stack "eksctl-basic-cluster-cluster"
2021-09-06 08:35:17 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:35:48 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:36:49 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:37:50 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:38:51 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:39:52 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:40:53 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:41:55 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:42:56 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:43:57 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:44:58 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:46:00 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:47:02 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:48:03 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-cluster"
2021-09-06 08:52:15 [ℹ]  building nodegroup stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:52:15 [ℹ]  --nodes-min=2 was set automatically for nodegroup ng-1
2021-09-06 08:52:15 [ℹ]  --nodes-max=2 was set automatically for nodegroup ng-1
2021-09-06 08:52:16 [ℹ]  deploying stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:52:16 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:52:33 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:52:51 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:53:11 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:53:29 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:53:50 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:54:12 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:54:31 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:54:49 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:55:08 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:55:26 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:55:43 [ℹ]  waiting for CloudFormation stack "eksctl-basic-cluster-nodegroup-ng-1"
2021-09-06 08:55:45 [ℹ]  waiting for the control plane availability...
2021-09-06 08:55:45 [✔]  saved kubeconfig as "/home/vagrant/.kube/config"
2021-09-06 08:55:45 [ℹ]  no tasks
2021-09-06 08:55:45 [✔]  all EKS cluster resources for "basic-cluster" have been created
2021-09-06 08:55:45 [ℹ]  adding identity "arn:aws:iam::724904243633:role/eksctl-basic-cluster-nodegroup-ng-NodeInstanceRole-1WDQPVD1HBCIZ" to auth ConfigMap
2021-09-06 08:55:46 [ℹ]  nodegroup "ng-1" has 0 node(s)
2021-09-06 08:55:46 [ℹ]  waiting for at least 2 node(s) to become ready in "ng-1"
2021-09-06 08:56:30 [ℹ]  nodegroup "ng-1" has 2 node(s)
2021-09-06 08:56:30 [ℹ]  node "ip-192-168-16-197.ec2.internal" is ready
2021-09-06 08:56:30 [ℹ]  node "ip-192-168-31-222.ec2.internal" is ready
2021-09-06 08:58:37 [ℹ]  kubectl command should work with "/home/vagrant/.kube/config", try 'kubectl get nodes'
2021-09-06 08:58:37 [✔]  EKS cluster "basic-cluster" in "us-east-1" region is ready

$ kubectl get nodes

NAME                             STATUS   ROLES    AGE    VERSION
ip-192-168-16-197.ec2.internal   Ready    <none>   5m2s   v1.20.7-eks-135321
ip-192-168-31-222.ec2.internal   Ready    <none>   5m1s   v1.20.7-eks-135321

$ kubectl -n kube-system get pods

NAME                       READY   STATUS    RESTARTS   AGE
aws-node-g659f             1/1     Running   0          21m
aws-node-z55mv             1/1     Running   0          21m
coredns-65bfc5645f-9zn9s   1/1     Running   0          32m
coredns-65bfc5645f-hc9f9   1/1     Running   0          32m
kube-proxy-hjxng           1/1     Running   0          21m
kube-proxy-kxvjg           1/1     Running   0          21m

$ eksctl create cluster -f ./cluster.yaml
```

Ref:

https://github.com/weaveworks/eksctl/tree/main/examples

https://github.com/weaveworks/eksctl

