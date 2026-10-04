# Security

## Security Goals

The migration introduced security controls around:

- container images
- secrets
- runtime privileges
- service-to-service communication
- deployment access

## Image Security

Container images were scanned before being published to Harbor, which was used as the private container registry.

This reduced the risk of promoting known vulnerable images into deployment environments.

## Secrets Management

Secrets were kept separate from ordinary application configuration.

An external secrets-management approach was used so that sensitive values did not need to be embedded directly in deployment manifests.

## Runtime Security

Workloads followed least-privilege principles where possible.

Practices included:

- non-root containers
- restricted service identities
- scoped access
- separation of configuration and secrets

## Network Isolation

Network policies were used to reduce unnecessary communication paths between workloads.

The intent was to move away from a flat network model toward explicit communication rules.

## Key Lesson

Kubernetes security is not one feature. It is a combination of image trust, identity, secrets, network controls and runtime configuration.
