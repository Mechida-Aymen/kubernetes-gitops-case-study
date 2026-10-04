🌐 **Language:** **English** | [Français](fr/security.md)

# Security

## Security Model

The security design combined image security, secret management, identity controls and network isolation.

## Image Security

Container images were scanned with Trivy before publication to Harbor.

## Harbor

Harbor acted as the controlled private image registry used by the Kubernetes environment.

## External Vault

HashiCorp Vault was deliberately deployed **outside the main Kubernetes cluster**.

This reduced dependence on the cluster itself for secret recovery and preserved access to sensitive configuration during a major Kubernetes outage.

Vault centralized values such as:

- application credentials
- database credentials
- authentication information
- service secrets

## External Secrets Operator

External Secrets Operator synchronized required values from Vault into Kubernetes Secret resources.

Applications could consume standard Kubernetes Secrets while the authoritative sensitive data remained centralized in Vault.

## RBAC & Service Accounts

Dedicated Service Accounts and scoped RBAC roles were used to limit permissions according to least-privilege principles.

## NetworkPolicies

Calico NetworkPolicies restricted unnecessary communication paths between workloads.

## Security Layers

```text
Artifact
  ↓
Docker Image
  ↓
Trivy Scan
  ↓
Harbor
  ↓
Kubernetes
  ├── RBAC / Service Accounts
  ├── NetworkPolicies
  └── External Secrets
          ↓
         Vault
```

## Key Lesson

Kubernetes security is a combination of trusted images, scoped identities, protected secrets, runtime configuration and network boundaries.
