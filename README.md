# DevSecOps-uploadServer


1. Creating a kind cluster


2. Deploying monitroing Stack:
A. Promtail:

helm repo add grafana https://grafana.github.io/helm-charts

helm upgrade --install promtail grafana/promtail \
  --namespace monitoring \
  --create-namespace \
  --set "loki.serviceName=loki" \
  --set "loki.servicePort=3100"


B. Loki

helm install loki grafana/loki \
  --namespace monitoring \
  --create-namespace \
  --version 6.29.0 \
  --values manifests/loki-values.yaml

C. Prometheus

helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace \
  --version 70.7.0 \
  --values manifests/prometheus-values.yaml


D. Falco

helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update

helm install falco falcosecurity/falco \
  --namespace falco --create-namespace \
  --values manifests/falco-values.yaml





4. Deploy Upload Server to Kubernetes

kubectl apply -f k8s/pv.yaml
kubectl apply -f k8s/pvc.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml


5. Run Initial Scans

A. Snyk Scan
firts login to the snyk

a. snyk auth

b. 


6. Helm Installing the manifests yaml files: prometheus, loki, grafana and falco:

a. Loki

b. 
