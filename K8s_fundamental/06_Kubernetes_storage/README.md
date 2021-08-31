# Kubernetes storage
## Volumes
https://kubernetes.io/docs/concepts/storage/

## ConfigMap
Provides a way to inject configuration data into Pod

Mount outside volume

deploy .yaml

Key/Value
  - Yaml
  - json
  - ...

Key -> file name
Value -> data

```
kubectl apply -R -f storage/configmap/

kubectl get configmap config-test
kubectl get cm

# check log & debug
kubectl describe cm

#Inside the Pod
cd /tmp/config 
watch cat yaml.config

ps axuw | grep bash
```

### view /proc
```
kubectl get pods -o wide

docker exec -it kind-worker bash
ps axuw | grep entrypoint

cd /proc/$PID

cat mountinfo | grep /etc/hosts
```

Modify the configmap and re-apply

## Secret
https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/
https://itnext.io/can-kubernetes-keep-a-secret-it-all-depends-what-tool-youre-using-498e5dee9c25

Contains a small amount of sensitive data
  - Password, token, key...

Two maps
  - data
    - Encode using base64
    - echo -n "xxx" | base64
    
  - string data


```
kubectl apply -R -f storage/secret

kubectl get secret
kubectl get pods
```

Kubectl exec pods
    - ls /tmp/config (for config-vol)
    - env (for config-env)

A little different from confimap

[note] tmpfs(is not written to disk) in Memory

```
kubectl krew install view-secrect
kubectl view-secret secret-test
kubectl view-secret secret-test -a
```

Reference secret as volume not env

```
# Encode
echo "yes" | base64
echo -n "yes" | base64

# Decode
echo -n "eWVz" | base64 -d
```

```
kubectl krew install view-secrect
kubectl view-secret secret-test
kubectl view-secret secret-test -a
```

## HostPath
https://kubernetes.io/docs/concepts/storage/volumes/#hostpath

https://kubernetes.io/docs/concepts/policy/pod-security-policy/

Install files into system (DeamonSet or NodeSelector)
  - Flannel
  - GPU

https://github.com/flannel-io/flannel/blob/master/Documentation/kube-flannel.yml

https://github.com/GoogleCloudPlatform/container-engine-accelerators/blob/master/daemonset.yaml


Node(tmp/data) to Pod
```

```

## NFS
Network File System

Install NFS Server

```
sudo apt-get install nfs-kernel-server
sudo mkdir /nfsshare
echo "/nfsshare *(rw,sync,no_root_squash)" | sudo tee /etc/exports
sudo exportfs -r
sudo showmount -e
```

Install NFS Client

```
docker exec -it kind-control-plane apt-get -y update
docker exec -it kind-control-plane apt-get -y install nfs-common
docker exec -it kind-worker apt-get -y update
docker exec -it kind-worker apt-get -y install nfs-common
docker exec -it kind-worker2 apt-get -y update
docker exec -it kind-worker2 apt-get -y install nfs-common
```

Multiple write command

```
while true; do echo $HOSTNAME >> /test/data ;sleep $[($RANDOM%5)+1]s; done
```

Access Permission

https://kubernetes.io/docs/concepts/storage/persistent-volumes/


## PV/PVC

Volume(ephemeral)

Persistent Volume (abstraction layer)
  - PV (Persistent Volume)
  - PVC (Persistent Volume Claim)

Pod -> PVC -> PV

```
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl describe pv
kubectl describe pvc
kubectl apply -f deploy.yaml
kubectl apply -f pvc-force.yaml
kubectl apply -f pvc-pending.yaml
kubectl describe pvc
```

## StorageClass
PV/PVC -> Static provisioning


### Dynamic Provisioning
https://kubernetes.io/docs/concepts/storage/storage-classes/#local

https://github.com/kubernetes-incubator/external-storage/tree/master/nfs-client

Pod -> PVC -> StorageClass -> PV -> Storage Provider

```
# Role Base Access Control
kubectl apply -f rbac.yaml

# Build NFS provisioner
kubectl apply -f deploy.yaml

# Deploy storageclass
kubectl apply -f sc.yaml

kubectl apply -f pod.yaml
kubectl apply -f pod-2.yaml
```

### NFS is StatefulSet
volumeClaimTemplate, not volumes in Yaml

```
sudo mkdir -p /nfsshare/sts-a
sudo mkdir -p /nfsshare/sts-b
sudo mkdir -p /nfsshare/sts-c

sudo touch /nfsshare/sts-a/a
sudo touch /nfsshare/sts-b/b
sudo touch /nfsshare/sts-c/c

kubectl apply -f pv.yaml
kubectl apply -f deploy.yaml
```

## Container Storage Interface (CSI)
https://kubernetes-csi.github.io/docs/drivers.html

https://github.com/kubernetes-csi/csi-driver-nfs

https://github.com/kubernetes/api/blob/master/core/v1/types.go#L177

https://github.com/container-storage-interface/spec/blob/master/spec.md

### Install storage driver solutions

Deployed the NFS CSI driver
  - DaemonSet + StatefulSet(no use in new version)

Deployed the PV/PVC + Pod

1. NFS server
2. DaemonSet for Node Plugin 

```
kubectl apply -f csi-nodeplugin-rbac.yaml

kubectl apply -f csi-nodeplugin-nfsplugin.yaml

kubectl apply -f csi-nfs-driverinfo.yaml

kubectl apply -f deploy.yaml
```

## Debug
