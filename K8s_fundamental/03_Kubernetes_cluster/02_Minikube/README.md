# 
## minikube
https://kubernetes.io/docs/tasks/tools/install-minikube/

```
# Install minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube

# If the new version of minikube is using kubernetes 1.18.0 or above, you need to install conntrack
sudo apt-get install conntrack
sudo ./minikube start --vm-driver=none 

sudo chown -R $USER $HOME/.kube $HOME/.minikube  

# Install kubectl  
sudo apt-get update && sudo apt-get install -y apt-transport-https curl

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubectl 

sudo ./minikube addons list

sudo ./minikube addons enable dashboard

kubectl port-forward --address 172.17.8.111 -n kubernetes-dashboard service/kubernetes-dashboard 8888:80
```

<img src="https://github.com/cly1213/K8s_labs/blob/main/image/minikube_dashboard.png"/>