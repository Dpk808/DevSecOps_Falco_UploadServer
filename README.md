# 🚀 DevSecOps Upload Server Project

This project demonstrates a complete **DevSecOps pipeline** for a Flask-based file upload server deployed on **Kubernetes**, integrated with monitoring, security scanning, and runtime threat detection.

---

## 📁 Project Structure

```
DevSecOps_Falco_UploadServer/
├── upload-server/            # Flask Upload App
├── k8s/                      # Kubernetes YAMLs (Deployment, Service, PVC)
├── manifests/                # Helm values for Loki, Prometheus, Falco
├── dashboards/               # Grafana dashboards
├── .github/workflows/        # GitHub Actions CI/CD pipelines
└── README.md                 # Project Documentation
```

---

## ⚙️ 1. Create a Kind Cluster

Install [Kind](https://kind.sigs.k8s.io/) and Docker, then run:

```bash
kind create cluster --name devsecops-cluster
```

![image alt](https://github.com/Dpk808/DevSecOps_Falco_UploadServer/blob/main/screenshots/1a.%20Creating%20a%20kind%20cluster.png) 


---

## 📦 2. Deploy Monitoring & Security Stack

### 🔹 A. Promtail

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm upgrade --install promtail grafana/promtail \
  --namespace monitoring \
  --create-namespace \
  --set "loki.serviceName=loki" \
  --set "loki.servicePort=3100"
```

### 🔹 B. Loki

```bash
helm install loki grafana/loki \
  --namespace monitoring \
  --create-namespace \
  --version 6.29.0 \
  --values manifests/loki-values.yaml
```

### 🔹 C. Prometheus Stack

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace \
  --version 70.7.0 \
  --values manifests/prometheus-values.yaml
```

### 🔹 D. Falco

```bash
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update

helm install falco falcosecurity/falco \
  --namespace falco \
  --create-namespace \
  --values manifests/falco-values.yaml
```

![image alt](1e. Helm Installing The yaml files of loki, prometheus and promtail.png) 


---

## 🚀 3. Deploy Upload Server to Kubernetes

```bash
kubectl apply -f k8s/pv.yaml
kubectl apply -f k8s/pvc.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

> 🔍 Access the service via `kubectl port-forward` or Ingress.

![image alt](https://github.com/Dpk808/DevSecOps_Falco_UploadServer/blob/main/screenshots/1c.%20Deploying%20Upload%20Server%20to%20Kubernetes.png) 


---

## 🔐 4. Run Security Scans

### ✅ A. Bandit (Python Code Scan)

```bash
pip install bandit
bandit -r upload-server/
```
![image alt](https://github.com/Dpk808/DevSecOps_Falco_UploadServer/blob/main/screenshots/1d%20Running%20Initial%20Bandit%20Scans.png) 


### ✅ B. Snyk (Dependency Scan)

```bash
npm install -g snyk
snyk auth  # login via browser
snyk test --file=upload-server/requirements.txt
```



---

## 🤖 5. GitHub Actions CI/CD

This repo uses a GitHub Actions workflow to:

* Run **Bandit**, **Snyk**, and **Trivy** scans
* Build and push Docker image to DockerHub
* Automatically deploy updated version to the cluster

### ✅ Trigger Deployment

Make changes and push to GitHub:

```bash
git add .
git commit -m "update logic"
git push origin main
```





GitHub Actions will handle:

* 🔒 Security scans
* 🐳 Docker build & push
* 🚀 Kubernetes deployment


![image alt](https://github.com/Dpk808/DevSecOps_Falco_UploadServer/blob/main/screenshots/2a.%20Deployment%20Done.png) 


---

## 📊 6. Monitoring & Logging (Grafana)

* **Prometheus**: Metrics from app and K8s
* **Loki + Promtail**: Application & Falco logs
* **Falco**: Runtime security alerts
* **Grafana**: Dashboards (`dashboards/*.json`)

---

## 🛠️ Tools Used

| Tool               | Purpose                            |
| ------------------ | ---------------------------------- |
| **Falco**          | Runtime Security Detection         |
| **Prometheus**     | Metrics Collection                 |
| **Grafana**        | Dashboard Visualization            |
| **Loki**           | Log Aggregation                    |
| **Promtail**       | Log Shipping                       |
| **Bandit**         | Static Code Analysis for Python    |
| **Snyk**           | Dependency Vulnerability Scanning  |
| **Trivy**          | Container Image Vulnerability Scan |
| **GitHub Actions** | CI/CD Pipeline                     |

---

## 📷 Example Dashboards

> 📌 Replace below with actual screenshot links in your repo later:

* ![Falco Alerts Dashboard](images/falco-dashboard.png)
* ![Upload Server Logs](images/secure-upload-server-dashboard.png)
* ![GitHub Actions Success](images/github-actions-success.png)

---

## 🧰 Prerequisites

* Docker
* Kind
* Helm
* Python 3.x
* GitHub & DockerHub accounts

---


