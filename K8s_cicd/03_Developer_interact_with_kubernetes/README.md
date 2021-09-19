# Container 
Should we have a local kubernetes?
 - Docker-compose

## Docker

```
sudo vim /lib/systemd/system/docker.service
#dockerd -H fd:// -H tcp://0.0.0.0:2375

sudo systemctl daemon-reload
sudo systemctl restart docker
sudo netstat -anlpt | grep 2375
```

```
#-H 's variable or DOCKER_HOST
docker -H=172.17.8.111:2375 info
DOCKER_HOST=172.17.8.111:2375 docker info
```

```
make DOCKER_HOST=172.17.8.111:2375 build-image

helm upgrade cicd --set nginx.image.tag=xxxxx --set.nginx.pullPolicy=IfNotPresent
```

## Kind

```
sudo docker exec -it kind-worker docker
sudo docker exec -it kind-worker crictl
```

```
helm install cicd helm
kubectl get svc
curl 172.18.0.3:$node_port

# Modify entrypoint.sh
vim entrypoint.sh
sudo make VERSION=1234567 build-image
sudo kind load docker-image hwchiu/cicd-app:1234567
helm upgrade --set nginx.image.tag=1234567 --set nginx.pullPolicy=IfNotPresent cicd helm
```