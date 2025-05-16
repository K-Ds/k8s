# Widgetario Kubernetes Hackathon Project

## Overview

This project is a hands-on implementation of the [Kubernetes Course Labs Hackathon](https://kubernetes.courselabs.co/hackathon/), designed to deepen understanding of Kubernetes by deploying a multi-service application called **Widgetario**.

The application comprises four core services:

* **products-api**: Java Spring Boot application providing product data.
* **products-db**: PostgreSQL database storing product information.
* **stock-api**: Go application managing stock levels.
* **web**: .NET Core frontend serving the user interface.

Additionally, the project includes logging and monitoring components to enhance observability.

## Project Structure

```
.
├── infrastructure/
│   ├── 01_namespace.yaml
│   ├── buildkitd.yaml
│   ├── gogs.yaml
│   └── jenkins.yaml
├── logging/
│   ├── elasticsearch.yaml
│   ├── fluentbit.yaml
│   ├── fluentbit-config.yaml
│   ├── kibana.yml
│   └── 01_namespace.yaml
├── monitoring/
│   ├── grafana.yaml
│   ├── grafana-config.yaml
│   ├── prometheus.yaml
│   ├── prometheus-config.yaml
│   └── 01_namespace.yaml
├── products-api/
│   ├── deployment.yaml
│   ├── ingress.yaml
│   ├── secrets.yaml
│   └── service.yaml
├── products-db/
│   ├── deployment.yaml
│   ├── secret.yaml
│   └── service.yaml
├── stock-api/
│   ├── deployment.yaml
│   ├── secret.yaml
│   └── service.yaml
└── web/
    ├── configMap.yaml
    ├── deployment.yaml
    ├── ingress.yaml
    ├── secret.yaml
    └── service.yaml
```

## Prerequisites

* [Docker](https://www.docker.com/)
* [k3d](https://k3d.io/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Cluster Setup

Create a local Kubernetes cluster using k3d:

```bash
k3d cluster create widgetario-k8s -p "80:80@loadbalancer"
```

## Hosts Configuration

To access the services via friendly domain names, add the following lines to your `/etc/hosts` file:

```
127.0.0.1 widgetario.local
127.0.0.1 api.widgetario.local
127.0.0.1 grafana.widgetario.local
127.0.0.1 kibana.widgetario.local
```

> ⚠️ **Note:** You will need administrative privileges to edit `/etc/hosts`.

## Deployment Instructions

### 1. Apply Namespaces and Infrastructure Components

```bash
kubectl apply -f infrastructure
```

### 2. Deploy Logging Stack

```bash
kubectl apply -f logging
```

### 3. Deploy Monitoring Stack

```bash
kubectl apply -f monitoring
```

### 4. Deploy Application Services

```bash
# products-db
kubectl apply -f products-db

# products-api
kubectl apply -f products-api

# stock-api
kubectl apply -f stock-api

# web
kubectl apply -f web
```

## Accessing the Application

Once deployed, the services can be accessed via:

* Web frontend: [http://widgetario.local](http://widgetario.local)
* Products API: [http://api.widgetario.local](http://api.widgetario.local)
* Grafana: [http://grafana.widgetario.local](http://grafana.widgetario.local)
* Kibana: [http://kibana.widgetario.local](http://kibana.widgetario.local)

## Configuration Notes

* **products-db**: Uses a secret for `POSTGRES_PASSWORD`.
* **stock-api**: Requires a database connection string (`POSTGRES_CONN`) set via a secret.
* **products-api**: Uses Spring Boot configuration and a Kubernetes secret for DB credentials.
* **web**: Configuration overridden by `/app/secrets/api.json`. Supports dark mode via `Widgetario__Theme=dark`.

## Observability

* **Logging**: Powered by Elasticsearch + Fluent Bit + Kibana.
* **Monitoring**: Powered by Prometheus + Grafana.

## CI/CD Integration

Includes Jenkins and Gogs setup for continuous integration workflows.

## Learning Objectives

By completing this project, you will:

* Deploy a multi-service app using Kubernetes
* Manage secrets, configs, ingress, and services
* Set up complete observability with logs and metrics
* Understand CI/CD workflows in Kubernetes environments

---
