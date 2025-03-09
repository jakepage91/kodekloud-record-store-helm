# KodeKloud Record Store Helm Chart

## Overview

This repository contains Helm charts for deploying the KodeKloud Record Store application on Kubernetes. The application consists of a main API service, a worker service, and several supporting services including PostgreSQL, RabbitMQ, and monitoring tools.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure

## Dependencies

This chart depends on several Bitnami and other community charts:

- PostgreSQL (for database)
- RabbitMQ (for message queue)
- Prometheus (for monitoring)
- Grafana (for visualization)
- Jaeger (for distributed tracing)

## Installation

1. Add the required Helm repositories:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm repo update
```

2. Update dependencies:

```bash
cd kodekloud-record-store-helm/kodekloud-record-store
helm dependency update
```

3. Install the chart:

```bash
helm install kodekloud-record-store ./kodekloud-record-store-helm/kodekloud-record-store
```


## Configuration

The following table lists the configurable parameters of the KodeKloud Record Store chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas for the API service | `1` |
| `image.repository` | Image repository | `jakepage91/kodekloud-record-store` |
| `image.tag` | Image tag | `latest` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `postgresql.enabled` | Enable PostgreSQL | `true` |
| `postgresql.auth.username` | PostgreSQL username | `admin` |
| `postgresql.auth.password` | PostgreSQL password | `password` |
| `postgresql.auth.database` | PostgreSQL database name | `kodekloud_records` |
| `rabbitmq.enabled` | Enable RabbitMQ | `true` |
| `prometheus.enabled` | Enable Prometheus | `true` |
| `grafana.enabled` | Enable Grafana | `true` |
| `jaeger.enabled` | Enable Jaeger | `true` |
| `worker.enabled` | Enable worker deployment | `true` |
| `worker.replicaCount` | Number of worker replicas | `1` |

## Architecture

The KodeKloud Record Store application consists of the following components:

1. **API Service**: The main application that handles HTTP requests.
2. **Worker Service**: Processes background tasks using Celery.
3. **PostgreSQL**: Stores application data.
4. **RabbitMQ**: Message broker for the worker service.
5. **Prometheus**: Collects and stores metrics.
6. **Grafana**: Visualizes metrics from Prometheus.
7. **Jaeger**: Distributed tracing system.

## Monitoring

The application is configured with a comprehensive monitoring stack:

- **Prometheus**: Scrapes metrics from the API and worker services.
- **AlertManager**: Handles alerts from Prometheus.
- **Grafana**: Provides dashboards for visualizing metrics.
- **Jaeger**: Collects and visualizes distributed traces.

## Customization

You can customize the deployment by overriding values in the `values.yaml` file. Create a custom values file:

```bash
helm install kodekloud-record-store ./kodekloud-record-store-helm/kodekloud-record-store -f my-values.yaml
```


## Troubleshooting

If you encounter issues with the deployment, check the following:

1. **Pod Status**: 
   ```bash
   kubectl get pods
   ```

2. **Pod Logs**:
   ```bash
   kubectl logs <pod-name>
   ```

3. **Service Connectivity**:
   ```bash
   kubectl exec -it <pod-name> -- curl <service-name>:<port>
   ```

4. **Persistent Volumes**:
   ```bash
   kubectl get pv,pvc
   ```

## Upgrading

To upgrade the chart:
```
helm upgrade kodekloud-record-store ./kodekloud-record-store-helm/kodekloud-record-store
```

## Uninstalling

To uninstall/delete the deployment:
```
helm uninstall kodekloud-record-store
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.