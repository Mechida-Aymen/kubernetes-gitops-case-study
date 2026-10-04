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

    JENKINS -->|update image tag| GIT
    ARGO --> K8S
    K8S -->|pull referenced image| HARBOR

    VAULT[HashiCorp Vault] --> ESO[External Secrets Operator]
    ESO --> K8S
```

## What the Diagram Shows

The project workflow separates:

- artifact retrieval and image build
- security scanning and image publication
- GitOps state management
- cluster reconciliation
- runtime image pulling

After pushing an image to Harbor, Jenkins automatically updated the image tag in the GitOps repository. Argo CD detected that Git change and synchronized Kubernetes, which then pulled the referenced image from Harbor.

The diagram is intentionally generic and excludes company-specific identifiers.
