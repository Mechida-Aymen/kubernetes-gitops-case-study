🌐 **Langue :** [English](../observability-flow.md) | **Français**

# Flux d’observabilité

```mermaid
flowchart LR
    NODE[Node Exporter] --> PROM[Prometheus]
    KSM[kube-state-metrics] --> PROM
    CAD[cAdvisor] --> PROM
    CASS[Cassandra Exporter] --> PROM
    JMX[JMX Exporter] --> PROM

    PROM --> GRAF[Dashboards Grafana]
    PROM --> RULES[Règles d'alerte]
    RULES --> AM[Alertmanager]
    AM --> MAIL[Notifications e-mail]
```

## Couverture

L’architecture collectait :

- les métriques des hôtes
- l’état des objets Kubernetes
- les métriques des conteneurs
- les métriques Cassandra
- les métriques JVM/applicatives

Prometheus centralisait la collecte et l’évaluation des alertes, Grafana assurait la visualisation et Alertmanager distribuait les alertes.
