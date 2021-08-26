## Rancher
https://rancher.com/

### Install Rancher Controller
```
docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher:v2.3.5

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
