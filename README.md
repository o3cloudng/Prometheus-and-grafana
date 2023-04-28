# SETUP prometheus AND GRAFANA IN K8S CLUSTER


## Before installing prometheus, you must install metric server

Deploy metric server:
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml
```

To verify that the metric server is running

```
kubectl get deployment metrics-server -n kube-system
```

## To Deploy prometheus
Create namespace prometheus
```
kubectl create namespace prometheus
```

Deploy prometheus using HELM chart

```How to install helm in k8s:```

```
<helm>
````

You can now use helm to install prometheus

```https://docs.aws.amazon.com/eks/latest/userguide/prometheus.html```
```
helm install prometheus stable/prometheus \
    --namespace prometheus \
    --set alertmanager.persistenVolume.storageClass="gp2", server.persistentVolume.storageClass="gp2"
```

```
kubectl get pods -n prometheus
```
Use kubectl to port forward the prometheus console to your local machine
```
kubectl --namespace=prometheus port-forward deploy/prometheus-server 9090
```

```http://localhost:9090```


# Grafana Graphs

Grafana works out of the box on prometheus (Opensource)

## Install grafana
Create a namespace for grafana
```
kubectl create namespace grafana
```

Install grafana using helm

```
helm install grafana stable/grafana \
    --namespace grafana \
    --set persistence.storageClassName="gp2" \
    --set persistence.enabled = true \
    --set adminPassword='@Password' \
    --set datasources."datasources\.yaml".appVersion=1 \
    --set datasources."datasources\.yaml".datasources[0].name=Prometheus \
    --set datasources."datasources\.yaml".datasources[0].type=Prometheus \
    --set datasources."datasources\.yaml".datasources[0].url=https://prometheus-server.prometheus \
    --set datasources."datasources\.yaml".datasources[0].access=proxy \
    --set datasources."datasources\.yaml".datasources[0].isDefault=true \
    --set service.type=LoadBalancer
```

Check if Grafana is deployed correctly
```
kubectl get all -n grafana
```