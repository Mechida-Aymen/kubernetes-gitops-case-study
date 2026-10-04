🌐 **Langue :** [English](../security.md) | **Français**

# Sécurité

## Modèle de sécurité

La conception combinait sécurité des images, gestion des secrets, contrôle des identités et isolation réseau.

## Sécurité des images

Les images de conteneurs étaient analysées avec Trivy avant publication dans Harbor.

## Harbor

Harbor jouait le rôle de registre privé contrôlé utilisé par l’environnement Kubernetes.

## Vault externe

HashiCorp Vault était volontairement déployé **hors du cluster Kubernetes principal**.

Ce choix réduisait la dépendance au cluster pour la récupération des secrets et permettait de conserver l’accès aux informations sensibles même lors d’une panne importante de Kubernetes.

Vault centralisait notamment :

- identifiants applicatifs
- identifiants de base de données
- informations d’authentification
- secrets de services

## External Secrets Operator

External Secrets Operator synchronisait les valeurs nécessaires depuis Vault vers des ressources Kubernetes Secret.

Les applications pouvaient donc utiliser les mécanismes standards Kubernetes tout en gardant la source de vérité des secrets dans Vault.

## RBAC & Service Accounts

Des Service Accounts dédiés et des rôles RBAC limités étaient utilisés selon le principe du moindre privilège.

## NetworkPolicies

Les NetworkPolicies Calico limitaient les communications inutiles entre workloads.

## Couches de sécurité

```text
Artefact
  ↓
Image Docker
  ↓
Scan Trivy
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

## Leçon principale

La sécurité Kubernetes repose sur la combinaison d’images fiables, d’identités limitées, de secrets protégés, de configuration runtime et de frontières réseau.
