# CI/CD & GitOps

## Delivery Architecture

The implementation separated **continuous integration** from **deployment reconciliation**.

```text
Nexus
  ↓
Jenkins
  ↓
Docker Build
  ↓
Trivy Scan
  ↓
Harbor
  ↓
GitOps Repository
  ↓
Argo CD
  ↓
Kubernetes
```

## Jenkins CI

Application artifacts were retrieved from Nexus.

Jenkins automated:

1. artifact retrieval
2. Docker image build
3. validation
4. Trivy vulnerability scanning
5. publication to Harbor

## Harbor

Harbor was used as the private image registry.

It centralized image storage and provided a controlled source for versioned container images consumed by Kubernetes.

## GitOps Repository

Kubernetes manifests and Helm configuration were stored in a dedicated Git repository representing the desired state of the platform.

## Argo CD

Argo CD continuously compared desired Git state with the real cluster state and synchronized approved changes.

This improved:

- traceability
- repeatability
- drift detection
- rollback capability
- separation of CI and deployment responsibilities

## Helm

Custom Helm charts were used to reduce repetitive YAML and generate reusable deployment resources.

Helm was especially important for the dynamic Cassandra topology.

## Key Lesson

The strongest improvement came from separating:

**artifact creation → image publication → desired deployment state → cluster reconciliation**
