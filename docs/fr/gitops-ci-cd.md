🌐 **Langue :** [English](../gitops-ci-cd.md) | **Français**

# CI/CD & GitOps

## Architecture de livraison

L’implémentation séparait la création des images de la réconciliation du déploiement.

```text
Artefact Nexus
      ↓
    Jenkins
      ↓
 Build Docker
      ↓
 Scan Trivy
      ↓
    Harbor
      ↓
Jenkins met à jour le tag image dans Git
      ↓
 Dépôt GitOps
      ↓
    Argo CD
      ↓
 Kubernetes
      ↓
Pull de l'image depuis Harbor
```

## Jenkins CI

Les artefacts applicatifs étaient récupérés depuis Nexus.

Jenkins automatisait :

1. la récupération de l’artefact
2. la construction de l’image Docker
3. les étapes de validation
4. le scan de vulnérabilités avec Trivy
5. la publication dans Harbor
6. la mise à jour du tag image déployé dans le dépôt GitOps

## Harbor

Harbor servait de registre privé d’images.

Il stockait les images validées et versionnées consommées par Kubernetes.

Harbor ne déclenchait **pas** le déploiement. La configuration Git référençait l’image Harbor.

## Dépôt GitOps

Les manifests Kubernetes et la configuration Helm étaient stockés dans un dépôt Git dédié représentant l’état désiré de la plateforme.

Après la publication d’une image, Jenkins mettait automatiquement à jour sa référence dans ce dépôt.

## Argo CD

Argo CD comparait en continu l’état désiré dans Git avec l’état réel du cluster.

Lorsqu’un nouveau tag image était commité, Argo CD détectait la modification et synchronisait les ressources Kubernetes concernées.

Kubernetes récupérait ensuite l’image référencée depuis Harbor.

## Pourquoi cette séparation est importante

- **Nexus** — artefacts applicatifs
- **Jenkins** — orchestration CI et mise à jour du tag image dans Git
- **Trivy** — analyse des vulnérabilités
- **Harbor** — stockage des images de conteneurs
- **Git** — état désiré
- **Argo CD** — réconciliation
- **Kubernetes** — exécution et pull des images

## Considération production

L’écriture automatique de Jenkins vers Git faisait partie du workflow du projet.

Un environnement de production peut introduire des mécanismes plus stricts : approbation par pull request, digests immuables, gates de promotion, signature d’images ou outils dédiés d’automatisation GitOps.

## Leçon principale

**Harbor stocke l’image ; Git stocke la référence désirée ; Argo CD réconcilie Git ; Kubernetes récupère l’image.**
