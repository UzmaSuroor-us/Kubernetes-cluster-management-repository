## Kubernetes Monitoring with Prometheus and Grafana

This project sets up a comprehensive monitoring system using **Prometheus** and **Grafana** to monitor the health and performance of a Kubernetes cluster.

## Overview

This project deploys **Prometheus** and **Grafana** on a Kubernetes cluster, collects metrics from various components like the Kubernetes API server, nodes, and pods, and visualizes the data on Grafana dashboards.

- **Prometheus** is used to collect and store metrics.
- **Grafana** is used to visualize the data collected by Prometheus.

## Features

- **Prometheus** collects Kubernetes metrics like:
  - API Server Metrics
  - Node Metrics
  - Pod Metrics
- **Grafana** is configured to display these metrics on customizable dashboards.

## Prerequisites

Before setting up the monitoring stack, ensure the following:

- A running **Kubernetes cluster**.
- **kubectl** configured to access the cluster.
- **Helm** installed for easy deployment of Prometheus and Grafana.

## Setup Instructions

### 1. Deploy Prometheus and Grafana using Helm

#### Install Prometheus

1. Add the Prometheus Helm chart repository:

   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
2. Install Prometheus in the monitoring namespace:

Copy code
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring

Install Grafana
Grafana will be automatically installed along with Prometheus when using the kube-prometheus-stack.

2. Access Grafana Dashboard
After the deployment, you can access the Grafana dashboard to visualize the metrics:

Get the Grafana admin password:

bash
Copy code
kubectl get secret prometheus-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 --decode
Forward the Grafana service to a local port:

bash
Copy code
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80
Open your browser and navigate to http://localhost:3000. Log in with the username admin and the password you retrieved.

3. Prometheus Configuration
The Prometheus configuration file (prometheus.yml) is used to scrape metrics from Kubernetes components. The configuration defines various scrape jobs to collect metrics from Kubernetes API servers, nodes, and pods.

4. Monitoring Targets
Prometheus scrapes metrics from various Kubernetes endpoints, including:

API Server: /metrics
Nodes: /metrics
Pods: /metrics
Grafana: Visualizes data from the Prometheus endpoints.
Metrics Collected
Prometheus collects metrics from the following Kubernetes components:

API Server: Collects metrics related to API server health and performance.
Nodes: Metrics about node resource usage.
Pods: Metrics related to pod health and resource usage.
Troubleshooting
If you're encountering issues, check the logs of Prometheus and Grafana using:

bash
Copy code
kubectl logs <pod-name> -n monitoring
