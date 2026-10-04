# Observability

## Goal

The monitoring design aimed to provide visibility across:

- Kubernetes infrastructure
- hosts and nodes
- application workloads
- JVM behavior
- Cassandra
- service availability

## Stack

The observability stack included:

- Prometheus
- Grafana
- Alertmanager
- infrastructure exporters
- JVM/application metrics
- Kubernetes metrics

## Metrics Collection

Prometheus collected metrics from the platform and monitored services.

Examples of monitored areas included:

- node resource usage
- pod health
- workload state
- application/JVM metrics
- Cassandra metrics
- service availability

## Dashboards

Grafana was used to turn metrics into operational views.

Useful dashboard categories included:

- cluster health
- application health
- CPU and memory
- JVM behavior
- database health
- replica status

## Alerting

Alertmanager handled alert routing from Prometheus.

The goal was to make alerts actionable by associating them with meaningful failure conditions instead of simply collecting raw metrics.

## Operational Principle

Observability was treated as part of the platform design, not as an optional layer added after deployment.
