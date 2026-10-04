🌐 **Language:** **English** | [Français](fr/observability.md)

# Observability

## Goal

Observability covered three main levels:

1. Kubernetes infrastructure
2. Cassandra
3. Java/Tomcat applications

## Core Stack

- Prometheus
- Grafana
- Alertmanager

## Kubernetes & Node Metrics

### Node Exporter

Collected host metrics such as CPU, memory, storage and network activity.

### kube-state-metrics

Exposed state information for Kubernetes resources such as Pods, Deployments, StatefulSets and Services.

### cAdvisor

Provided container-level resource metrics.

## Cassandra Monitoring

A Cassandra Exporter exposed database-oriented metrics including node state, read/write behavior, latency and replication indicators.

## Application & JVM Monitoring

A JMX Exporter was used for Java/Tomcat metrics, including:

- memory usage
- garbage collection
- thread counts
- runtime/application behavior

## Prometheus

Prometheus centralized metric collection and evaluated alert rules.

## Grafana

Grafana dashboards provided views for:

- cluster health
- Cassandra behavior
- application/JVM metrics
- resource consumption

## Alertmanager

Alertmanager centralized alerts, applied grouping/filtering and distributed notifications.

Email was used as an alert delivery mechanism in the documented environment.

## Operational Principle

Monitoring was designed as part of the platform architecture rather than added after deployment.
