#
## KIND(Kubernetes in Docker)
https://kind.sigs.k8s.io/docs/user/quick-start/

```
curl -Lo ./kind "https://github.com/kubernetes-sigs/kind/releases/download/v0.7.0/kind-$(uname)-amd64"
chmod +x ./kind

sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update
sudo apt-get install -y kubectl   

sudo ./kind create cluster --config hiskio-course/vagrant/kind.yaml

sudo chown -R $USER $HOME/.kube

docker exec -it kind-control-plane docker
docker exec -it kind-control-plane bash

docker exec kind-control-plane crictl ps
``` 
