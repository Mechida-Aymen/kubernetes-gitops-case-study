🌐 **Language:** **English** | [Français](fr/observability-flow.md)

# Observability Flow

```mermaid
flowchart LR
    NODE[Node Exporter] --> PROM[Prometheus]
    KSM[kube-state-metrics] --> PROM
    CAD[cAdvisor] --> PROM
    CASS[Cassandra Exporter] --> PROM
    JMX[JMX Exporter] --> PROM

    PROM --> GRAF[Grafana Dashboards]
    PROM --> RULES[Alert Rules]
    RULES --> AM[Alertmanager]
    AM --> MAIL[Email Notifications]
```

## Coverage

The monitoring architecture collected:

- host metrics
- Kubernetes object state
- container metrics
- Cassandra metrics
- JVM/application metrics

Prometheus centralized collection and alert evaluation, Grafana handled visualization, and Alertmanager handled alert distribution.
