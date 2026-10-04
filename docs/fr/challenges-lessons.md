🌐 **Langue :** [English](../challenges-lessons.md) | **Français**

# Défis & Retours d’expérience

## 1. Les applications legacy transportent des hypothèses liées aux hôtes

La migration a nécessité d’identifier les hypothèses concernant la configuration locale, les attentes réseau, les chemins de déploiement et le comportement au démarrage.

La conteneurisation seule ne supprime pas ces contraintes.

## 2. Le réseau Kubernetes demande une conception attentive

Le trafic du cluster, l’Ingress, la découverte Cassandra et les flux de monitoring avaient des besoins différents.

Calico, les Services ClusterIP, les Headless Services, NGINX Ingress et le reverse proxy externe répondaient chacun à un besoin distinct.

## 3. Les workloads stateful sont fondamentalement différents

Cassandra nécessitait une identité stable, un stockage persistant, un placement contrôlé, une topologie explicite et un bootstrap automatisé.

## 4. La génération Helm dynamique réduit la répétition

La génération de la topologie Cassandra à partir des valeurs Helm réduisait la duplication des ressources et simplifiait la gestion des Data Centers logiques.

## 5. L’initialisation doit être automatisée

Le schéma, les données initiales et les utilisateurs étaient initialisés automatiquement afin de rendre les déploiements cohérents et reproductibles.

## 6. Startup, Readiness et Liveness répondent à des problèmes différents

Ces sondes couvrent des étapes distinctes du cycle de vie d’un workload et ne doivent pas être interchangeables.

## 7. GitOps réduit les changements invisibles

Conserver l’état désiré dans Git améliore la traçabilité et rend le drift de configuration visible.

## 8. Les secrets externes améliorent la séparation des responsabilités

Vault restait hors du cluster tandis qu’External Secrets Operator gérait la synchronisation vers Kubernetes.

## 9. L’observabilité nécessite plusieurs perspectives

Un monitoring utile devait combiner métriques infrastructure, ressources Kubernetes, conteneurs, Cassandra et JVM.

## 10. Une migration ne se limite pas à la conteneurisation

Le projet a transformé les workflows de livraison, la gestion des configurations, le réseau, les secrets, le monitoring, le scaling et la reprise.
