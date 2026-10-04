🌐 **Language:** **English** | [Français](fr/gitops-ci-cd.md)

# CI/CD & GitOps

## Delivery Architecture

The implementation separated image creation from deployment reconciliation.

```text
Nexus Artifact
      ↓
    Jenkins
      ↓
 Docker Build
      ↓
 Trivy Scan
      ↓
    Harbor
      ↓
Jenkins updates image tag in Git
      ↓
 GitOps Repository
      ↓
    Argo CD
      ↓
 Kubernetes
      ↓
Pull image from Harbor
```

## Jenkins CI

Application artifacts were retrieved from Nexus.

Jenkins automated:

1. artifact retrieval
2. Docker image build
3. validation
4. Trivy vulnerability scanning
5. publication to Harbor
6. update of the deployed image tag in the GitOps repository

## Harbor

Harbor was used as the private image registry.

It stored the validated, versioned container images consumed by Kubernetes.

Harbor did **not** trigger deployment. The deployment configuration in Git referenced the Harbor image.

## GitOps Repository

Kubernetes manifests and Helm configuration were stored in a dedicated Git repository representing the desired state of the platform.

After a successful image publication, Jenkins automatically updated the image reference in this repository.

## Argo CD

Argo CD continuously compared the desired state in Git with the real cluster state.

When Jenkins committed a new image tag, Argo CD detected the Git change and synchronized the related Kubernetes resources.

Kubernetes then pulled the referenced image from Harbor.

## Why This Separation Matters

The architecture still kept clear roles:

- **Nexus** — application artifacts
- **Jenkins** — CI orchestration and Git image-tag update
- **Trivy** — vulnerability scanning
- **Harbor** — container image storage
- **Git** — desired deployment state
- **Argo CD** — reconciliation
- **Kubernetes** — runtime execution and image pull

## Production Consideration

The project used automated Jenkins write-back to the GitOps repository.

That was suitable for the lab/project workflow, but production environments may require stricter promotion mechanisms such as pull-request approval, immutable image digests, environment gates, signed images or dedicated GitOps image automation.

## Key Lesson

The important distinction is:

**Harbor stores the image; Git stores the desired image reference; Argo CD reconciles Git; Kubernetes pulls the image.**
