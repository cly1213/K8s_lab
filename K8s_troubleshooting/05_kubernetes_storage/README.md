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

## 