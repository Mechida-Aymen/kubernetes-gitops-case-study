🌐 **Langue :** [English](../scalability-ha.md) | **Français**

# Scalabilité & Haute Disponibilité

## Workloads applicatifs répliqués

Les workloads applicatifs stateless étaient déployés avec plusieurs réplicas.

Kubernetes pouvait recréer les pods défaillants et maintenir la disponibilité sans dépendre d’une seule instance.

## Sondes de santé

### Startup Probe

Utilisée pour les applications nécessitant un temps d’initialisation plus long afin d’éviter des redémarrages prématurés.

### Readiness Probe

Déterminait si un pod était prêt à recevoir du trafic.

### Liveness Probe

Détectait les instances bloquées ou défaillantes afin que Kubernetes puisse les redémarrer.

## Horizontal Pod Autoscaler

Le HPA était utilisé pour la couche applicative.

Le scaling prenait en compte l’utilisation CPU et mémoire afin d’ajuster le nombre de réplicas selon la charge.

## Disponibilité Cassandra

Cassandra utilisait des StatefulSets car chaque nœud nécessitait une identité stable et un état persistant.

Les mécanismes comprenaient :

- plusieurs Data Centers logiques
- volumes persistants
- Node Affinity
- Pod Anti-Affinity
- Headless Services
- initialisation automatisée

## Prise en compte des domaines de panne

Les nœuds Cassandra étaient répartis entre plusieurs Workers lorsque possible afin de réduire l’impact de la panne d’un seul nœud.

## Principe de haute disponibilité

La haute disponibilité était abordée comme une combinaison de :

- redondance
- connaissance de l’état de santé
- recréation automatique
- routage tenant compte de la readiness
- scaling horizontal
- persistance
- placement tenant compte de la topologie
- monitoring
