🌐 **Langue :** [English](../high-level-architecture.md) | **Français**

# Vue d’architecture

L’architecture est volontairement séparée en deux vues afin d’éviter un diagramme trop chargé.

## 1. Livraison & GitOps

```mermaid
flowchart LR
    NEXUS[Nexus] --> JENKINS[Jenkins]
    JENKINS --> BUILD[Build Docker]
    BUILD --> TRIVY[Scan Trivy]
    TRIVY --> HARBOR[Harbor]

    JENKINS -->|mise à jour du tag image| GIT[Dépôt GitOps]
    GIT --> ARGO[Argo CD]
    ARGO --> K8S[Kubernetes]

    K8S -.->|pull de l'image| HARBOR
```

## 2. Plateforme d’exécution

```mermaid
flowchart TB
    USERS[Utilisateurs] --> RP[Reverse Proxy externe]
    RP --> ING[NGINX Ingress]
    ING --> APP[Deployments Java / Tomcat]

    APP --> CASS[StatefulSets Cassandra]

    VAULT[HashiCorp Vault] --> ESO[External Secrets Operator]
    ESO --> APP

    APP --> PROM[Prometheus]
    CASS --> PROM
    PROM --> GRAF[Grafana]
    PROM --> ALERT[Alertmanager]
```

Les diagrammes restent volontairement génériques et excluent les identifiants spécifiques à l’entreprise.
