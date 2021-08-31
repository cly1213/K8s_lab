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
