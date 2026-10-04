🌐 **Langue :** [English](../architecture.md) | **Français**

# Architecture

## Contexte legacy

Le point de départ était une plateforme basée sur des machines virtuelles, composée d’applications Java/Spring hébergées sur Tomcat et d’une couche de données Cassandra distribuée.

Les opérations dépendaient fortement de configurations serveur et de procédures manuelles pour le déploiement, le scaling, l’initialisation et la reprise.

## Environnement de validation

Le laboratoire documenté utilisait Kubernetes installé avec kubeadm sur des machines virtuelles :

| Nœud | CPU | RAM | Rôle |
|---|---:|---:|---|
| Control Plane | 3 vCPU | 6 Go | Gestion du cluster |
| Worker 1 | 3 vCPU | 6 Go | Workloads |
| Worker 2 | 3 vCPU | 6 Go | Workloads |
| Worker 3 | 3 vCPU | 6 Go | Workloads |

Les nœuds exécutaient Ubuntu Server 22.04 LTS et Kubernetes v1.33.

Calico assurait le réseau des pods et supportait les NetworkPolicies.

## Conception des workloads

### Couche applicative

Les applications Java/Tomcat étaient déployées sous forme de Deployments Kubernetes répliqués.

La configuration était externalisée via ConfigMaps et l’accès interne était assuré par des Services ClusterIP.

### Couche de données

Cassandra était déployé avec des StatefulSets car une identité stable et un stockage persistant étaient nécessaires.

La conception intégrait également des Headless Services, un placement tenant compte de la topologie et un bootstrap automatisé.

## Architecture réseau

Le trafic externe suivait le chemin suivant :

```text
Utilisateurs
  ↓
Reverse Proxy externe
  ↓
NGINX Ingress Controller
  ↓
Service ClusterIP
  ↓
Pods applicatifs
```

Cassandra utilisait un Headless Service afin de permettre la résolution DNS directe de chaque nœud pour la découverte, la réplication et la communication intra-ring.

Une interface réseau dédiée était également utilisée pour les communications liées au cluster dans l’environnement de lab.

## Ordonnancement

Node Affinity et Pod Anti-Affinity étaient utilisés pour répartir les nœuds Cassandra entre les Workers et réduire la concentration des risques.

## Principe de conception

Le principe central était de remplacer autant que possible les procédures opérationnelles spécifiques aux hôtes par un comportement déclaratif de la plateforme.
