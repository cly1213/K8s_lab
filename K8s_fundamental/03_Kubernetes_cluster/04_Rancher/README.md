# 
##
https://rancher.com/

### Install Rancher Controller
```
docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher:v2.3.5

docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher:v2.3.5
Unable to find image 'rancher/rancher:v2.3.5' locally
v2.3.5: Pulling from rancher/rancher
5c939e3a4d10: Pull complete
c63719cdbe7a: Pull complete
19a861ea6baf: Pull complete
651c9d2d6c4f: Pull complete
6d1c86a401db: Pull complete
c7d485afd256: Pull complete
285d247a5c2b: Pull complete
e7acc0299fc2: Pull complete
7346f9f2da73: Pull complete
91f1fe4c3d21: Pull complete
8b36bf060d06: Pull complete
1e46e34177f8: Pull complete
08aff7247104: Pull complete
bf9b23e67888: Pull complete
Digest: sha256:cdffc4e0d1aff6adb606b0f9f3033bf7667bcda69aa43293355b6bc2701b5c6a
Status: Downloaded newer image for rancher/rancher:v2.3.5
fbb9fd61474162dd5503ffa87b7135a9ef9c7b389f0d275a038e2281b70dd84f

docker ps
CONTAINER ID   IMAGE                    COMMAND           CREATED          STATUS          PORTS                                      NAMES
fbb9fd614741   rancher/rancher:v2.3.5   "entrypoint.sh"   54 seconds ago   Up 51 seconds   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   loving_liskov

ip addr | grep 111
    inet 172.17.8.111/24 brd 172.17.8.255 scope global eth1

```
Open browser

```
https://172.17.8.111
```
<img src="https://github.com/cly1213/K8s_labs/blob/main/image/rancher_dashboard.png"/>
