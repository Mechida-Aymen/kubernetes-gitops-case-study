🌐 **Langue :** [English](../cicd-gitops-flow.md) | **Français**

# Flux CI/CD & GitOps

```mermaid
flowchart LR
    NEXUS[Artefact Nexus]
      --> JENKINS[Pipeline Jenkins]
      --> BUILD[Build Docker]
      --> TRIVY[Scan Trivy]
      --> HARBOR[Push vers Harbor]

    JENKINS -->|mise à jour du tag image| GIT[Dépôt GitOps]
    GIT --> ARGO[Argo CD]
    ARGO --> K8S[Kubernetes]

    K8S -.->|pull de l'image référencée| HARBOR
```

## Déroulement

1. Jenkins récupère l’artefact depuis Nexus.
2. Jenkins construit l’image.
3. Trivy analyse l’image.
4. Jenkins publie l’image validée dans Harbor.
5. Jenkins commit automatiquement le nouveau tag image dans le dépôt GitOps.
6. Argo CD détecte le changement Git.
7. Argo CD synchronise Kubernetes.
8. Kubernetes récupère l’image référencée depuis Harbor.
