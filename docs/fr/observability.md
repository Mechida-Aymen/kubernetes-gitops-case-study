🌐 **Langue :** [English](../observability.md) | **Français**

# Observabilité

## Objectif

L’observabilité couvrait trois niveaux principaux :

1. infrastructure Kubernetes
2. Cassandra
3. applications Java/Tomcat

## Stack principale

- Prometheus
- Grafana
- Alertmanager

## Métriques Kubernetes & nœuds

### Node Exporter

Collecte de métriques système : CPU, mémoire, stockage et activité réseau.

### kube-state-metrics

Exposition de l’état des ressources Kubernetes : Pods, Deployments, StatefulSets et Services.

### cAdvisor

Métriques détaillées au niveau des conteneurs.

## Monitoring Cassandra

Un Cassandra Exporter exposait des métriques liées à l’état des nœuds, aux lectures/écritures, à la latence et à la réplication.

## Monitoring applicatif & JVM

Un JMX Exporter était utilisé pour les métriques Java/Tomcat :

- utilisation mémoire
- activité du garbage collector
- nombre de threads
- comportement runtime/applicatif

## Prometheus

Prometheus centralisait la collecte et évaluait les règles d’alerte.

## Grafana

Les dashboards Grafana permettaient de visualiser :

- la santé du cluster
- le comportement de Cassandra
- les métriques JVM/applicatives
- la consommation des ressources

## Alertmanager

Alertmanager centralisait les alertes, appliquait le regroupement/filtrage et distribuait les notifications.

Les notifications e-mail étaient utilisées dans l’environnement documenté.

## Principe opérationnel

Le monitoring a été conçu comme une partie intégrante de l’architecture, et non comme un ajout après déploiement.
