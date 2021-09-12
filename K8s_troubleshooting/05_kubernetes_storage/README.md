# Storage

## Workload
- pod -> storageProvider
- Pod -> PVC -> PV -> storageProvider
- Pod -> PVC -> storageClass -> PV -> storageProvider (Dynamic)

PV (沒有分namespace) 

cat pv.yaml

```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: nfs-demo
spec:
  storageClassName: pv-demo 
  capacity:
    storage: 10Mi
  accessModes:
    - ReadWriteOnce
  nfs:
    server: 172.18.0.1
    path: "/nfsshare"
```

PVC (有分namespace)

cat pvc.yaml

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: nfs-demo
spec:
  storageClassName: pv-demo
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Mi
```

cat deployment-nfs-pvc.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nfs-pv-pvc
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nfs-pv-pvc
  template:
    metadata:
      labels:
        app: nfs-pv-pvc
    spec:
      containers:
      - image: hwchiu/netutils
        name: c1
        volumeMounts:
          - mountPath: "/c1"
            name: nfs-demo
      volumes:
        - name: nfs-demo
          persistentVolumeClaim:
            claimName: nfs-demo
```

透過claim來指定pv

```
#  claimRef:
#    name: nfs-claimref-demo
#    namespace: default
```

使用emptydir -> 相同pod裡的container使用共同的空間，若pod死掉資料也跟著清空 -> 注意使用

## ConfigMap/Secret
update configmap

```
kubectl get pods configmap-demo-745bdb7746-9zwvx -o json | jq '.metadata.uid'

# 觀察 /var/lib/kubelet/pods/xxxxx 資料夾，看看這些 pod 裡面的資料長怎樣，包含 hosts, volumes, volumes-subpath

# 可以用 watch 指令定期觀察
``` 

volume跟著ConfigMap走

subpath跟著Container走，不會自動更新 -> 需要Pod重啟才會

### Auto Reload
https://github.com/stakater/Reloader

```
kubectl apply -f https://raw.githubusercontent.com/stakater/Reloader/master/deployments/kubernetes/reloader.yaml

# 之後重新修改 configMap 並更新，可以觀察到有打入 Annotation 的 Deployment 會馬上被影響，相關的服務都會重起!

# Rolling update!!!
```

## Local Volume
### Hostpath
Mount a file or directory from the host into your pod

重啟後可以會換在不同node上

Wait for first consumer -> 等待使用後在綁定pv

### Local

Deploy storageClass

local-storage

固定在特一node上:

1. storageClass + hostpath

2. local volume


