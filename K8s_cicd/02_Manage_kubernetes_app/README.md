# Kubernetes Applications
Defined by yaml files

## Helm
https://helm.sh/

### The package manager for Kubernetes

Helm like as apt-get install

Helm => K8s packages

Helm Chart(many yaml): Templates/Values

Using Helm 3

```
$ sudo snap install helm --classic
2021-09-16T23:29:34Z INFO Waiting for restart...
helm 3.7.0 from Snapcrafters installed

$ helm version
version.BuildInfo{Version:"v3.7.0", GitCommit:"eeac83883cb4014fe60267ec6373570374ce770b", GitTreeState:"clean", GoVersion:"go1.16.8"}
```

helm chart example

```
$ helm repo add stable https://charts.helm.sh/stable
"stable" has been added to your repositories

$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈

$ helm install stable/mysql --generate-name
WARNING: This chart is deprecated
NAME: mysql-1631836312
LAST DEPLOYED: Thu Sep 16 23:51:53 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
mysql-1631836312.default.svc.cluster.local

To get your root password run:

    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql-1631836312 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

To connect to your database:

1. Run an Ubuntu pod that you can use as a client:

    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

2. Install the mysql client:

    $ apt-get update && apt-get install mysql-client -y

3. Connect using the mysql cli, then provide your password:
    $ mysql -h mysql-1631836312 -p

To connect to your database directly from outside the K8s cluster:
    MYSQL_HOST=127.0.0.1
    MYSQL_PORT=3306

    # Execute the following command to route the connection:
    kubectl port-forward svc/mysql-1631836312 3306

    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}

$ helm ls
NAME            	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART      	APP VERSION
mysql-1631836312	default  	1       	2021-09-16 23:51:53.645970579 +0000 UTC	deployed	mysql-1.6.9	5.7.30
```

```
$ helm create test

$ cd test
$ helm install test .
NAME: test
LAST DEPLOYED: Thu Sep 16 23:49:32 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=test,app.kubernetes.io/instance=test" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT

```

```
helm search repo stable

helm install test --dry-run --set nginx.replicaCount=10 helm

```


