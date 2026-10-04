# CI/CD & GitOps

## Separation of Responsibilities

The delivery model intentionally separated **continuous integration** from **continuous deployment**.

CI was responsible for producing trusted application artifacts.

GitOps was responsible for defining and reconciling deployment state.

## CI Flow

A typical CI flow was:

1. retrieve source code
2. build the application
3. build the container image
4. scan the image
5. publish the image to Harbor

This meant that deployment systems consumed versioned artifacts rather than rebuilding applications during deployment.

## GitOps Flow

Argo CD monitored the Git repository containing the desired Kubernetes deployment state.

The flow was:

1. a deployment change was committed to Git
2. Argo CD detected the change
3. desired state was compared with cluster state
4. the application was synchronized
5. drift became visible through GitOps reconciliation

## Why GitOps Was Useful

GitOps improved:

- auditability
- rollback capability
- change visibility
- environment consistency
- separation between build and deployment
- operational repeatability

## Helm

Helm was used to package deployment configuration and avoid duplicating large amounts of Kubernetes YAML.

This helped parameterize:

- replica counts
- images
- resources
- ingress values
- environment-specific settings

## Key Lesson

A major improvement came from treating Git as the source of truth for deployment intent instead of relying on manual cluster changes.
