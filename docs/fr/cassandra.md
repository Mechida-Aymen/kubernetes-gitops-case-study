🌐 **Langue :** [English](../cassandra.md) | **Français**

# Cassandra sur Kubernetes

## Pourquoi Cassandra nécessitait des workloads stateful

La plateforme legacy utilisait un cluster Cassandra distribué. La migration devait préserver plusieurs caractéristiques propres aux systèmes stateful :

- identité stable des nœuds
- stockage persistant
- découverte du cluster
- prise en compte de la topologie
- bootstrap contrôlé
- placement explicite

Les StatefulSets Kubernetes ont été utilisés car des Deployments stateless classiques n’offraient pas les mêmes garanties d’identité et de stockage.

## Topologie Helm dynamique

Un chart Helm personnalisé a été développé depuis zéro afin de générer dynamiquement la topologie Cassandra à partir de valeurs.

Exemple simplifié et anonymisé :

```yaml
datacenters:
  - name: dc1
    replicas: 1
    nodeLabel: "01"
  - name: dc2
    replicas: 1
    nodeLabel: "02"
  - name: dc3
    replicas: 1
    nodeLabel: "03"
```

À partir de ces valeurs, Helm générait un StatefulSet par Data Center logique.

## Stockage persistant

Chaque pod Cassandra recevait son propre volume persistant via les volume claim templates des StatefulSets.

Une StorageClass permettait la création automatique des volumes nécessaires.

## Découverte de service

Un Headless Service était utilisé pour Cassandra afin de permettre la résolution DNS directe de chaque nœud.

Cela facilitait :

- la découverte des nœuds
- la communication intra-ring
- la réplication
- la synchronisation

## Placement

Node Affinity permettait d’associer les nœuds Cassandra à la stratégie de placement prévue.

Pod Anti-Affinity réduisait le risque de placer des nœuds équivalents sur le même Worker.

## Bootstrap automatisé

Un mécanisme post-install était intégré au déploiement Helm.

Il automatisait notamment :

- la création des keyspaces nécessaires
- l’exécution du schéma
- le chargement des données initiales
- la création des utilisateurs applicatifs
- la suppression de l’utilisateur Cassandra par défaut

## Leçon principale

Une migration stateful ne se limite pas au stockage persistant.

L’identité, la découverte, la topologie, le placement et l’initialisation doivent tous être représentés explicitement.
