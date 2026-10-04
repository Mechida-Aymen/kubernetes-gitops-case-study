# High-Level Architecture

```mermaid
flowchart TB
    subgraph Delivery["CI / Artifact Delivery"]
      NEXUS[Nexus Artifacts]
      JENKINS[Jenkins]
      BUILD[Docker Build]
      TRIVY[Trivy Scan]
      HARBOR[Harbor Registry]
      NEXUS --> JENKINS --> BUILD --> TRIVY --> HARBOR
    end

    subgraph GitOps["GitOps"]
      GIT[Git Repository]
      ARGO[Argo CD]
      GIT --> ARGO
    end

    subgraph Access["External Access"]
      USERS[Users]
      RP[Reverse Proxy]
      INGRESS[NGINX Ingress]
      USERS --> RP --> INGRESS
    end

    subgraph K8S["Kubernetes Cluster"]
      APP[Java / Tomcat Deployments]
      CASS[Cassandra StatefulSets]
      SERVICES[ClusterIP / Headless Services]
      PROM[Prometheus]
      GRAF[Grafana]
      ALERT[Alertmanager]

      INGRESS --> APP
      SERVICES --> APP
      SERVICES --> CASS
      PROM --> GRAF
      PROM --> ALERT
    end

    HARBOR --> K8S
    ARGO --> K8S

    VAULT[HashiCorp Vault] --> ESO[External Secrets Operator]
    ESO --> K8S
```

## What the Diagram Shows

The architecture separates four responsibilities:

- artifact and image delivery
- GitOps reconciliation
- application/data workloads
- security and observability

The diagram is intentionally generic and excludes company-specific identifiers.
