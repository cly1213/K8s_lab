# 
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
#-H 的變數 or DOCKER_HOST
docker -H=172.17.8.111:2375 info
DOCKER_HOST=172.17.8.111:2375 docker info
```

```
make DOCKER_HOST=172.17.8.111:2375 build-image

helm upgrade cicd --set nginx.image.tag=xxxxx --set.nginx.pullPolicy=IfNotPresent
```