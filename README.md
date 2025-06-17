# DevSecOps-uploadServer


1. Creating a kind cluster


2. Deploying monitroing Stack:

A. Prometheus

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm upgrade --install prometheus prometheus-community/prometheus \
  --values manifests/prometheus-values.yaml

B. Loki

helm repo add grafana https://grafana.github.io/helm-charts
helm upgrade --install loki grafana/loki \
  --values manifests/loki-values.yaml

C. Grafana

helm upgrade --install grafana grafana/grafana \
  --set adminPassword='admin' \
  --set service.type=NodePort

D. Promtail


kubectl apply -f manifests/promtail-deployment.yaml  


3. Deploy Falco for Runtime Threat Detection

helm repo add falcosecurity https://falcosecurity.github.io/charts

helm upgrade --install falco falcosecurity/falco \
  --values manifests/falco-values.yaml


4. Deploy Upload Server to Kubernetes

kubectl apply -f k8s/pv.yaml
kubectl apply -f k8s/pvc.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

