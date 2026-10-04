# CI/CD & GitOps Flow

```mermaid
flowchart LR
    A[Nexus Artifact] --> B[Jenkins Pipeline]
    B --> C[Docker Image Build]
    C --> D[Trivy Vulnerability Scan]
    D --> E{Scan Accepted?}
    E -- No --> F[Stop / Review]
    E -- Yes --> G[Push to Harbor]

    H[GitOps Repository] --> I[Argo CD]
    I --> J[Compare Desired vs Actual State]
    J --> K[Sync Kubernetes Resources]

    G --> K
```

## Separation of Responsibilities

The flow deliberately separates:

**CI**
- retrieve artifact
- build image
- scan image
- publish image

from:

**GitOps/CD**
- store desired deployment state in Git
- detect drift
- synchronize approved state with Kubernetes

This separation improves traceability and reduces hidden manual changes.
