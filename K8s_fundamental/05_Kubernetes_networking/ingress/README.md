Install nginx ingress

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/mandatory.yaml


kindIP=$(docker inspect kind-worker  | jq '.[0].NetworkSettings.Networks.bridge.IPAddress' | tr -d '""')

echo "$kindIP test.com" | sudo tee -a  /etc/hosts

echo "$kindIP hello.com" | sudo tee -a  /etc/hosts

echo "$kindIP httpd.com" | sudo tee -a  /etc/hosts

NODEPORT=$(kubectl -n ingress-nginx get svc ingress-nginx  -o jsonpath='{.spec.ports[0].nodePort}')

curl test.com:$NODEPORT/v1/
curl test.com:$NODEPORT/v2/
curl test.com:$NODEPORT/v3

curl hello.com:$NODEPORT
curl httpd.com:$NODEPORT
```

