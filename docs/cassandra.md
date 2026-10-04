🌐 **Language:** **English** | [Français](fr/cassandra.md)

# Cassandra on Kubernetes

## Why Cassandra Needed Stateful Workloads

The legacy platform used a distributed Cassandra cluster. Migrating this layer required preserving the characteristics that make Cassandra stateful:

- stable node identity
- persistent storage
- cluster discovery
- topology awareness
- controlled bootstrap
- explicit placement

Kubernetes StatefulSets were used because ordinary stateless Deployments would not provide the same identity and storage guarantees.

## Dynamic Helm Topology

A custom Helm chart was developed from scratch to generate the Cassandra topology from values.

A simplified sanitized example:

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

From these values, Helm generated one StatefulSet per logical Data Center.

This design made it easier to add or remove Data Centers without rewriting large sets of manifests.

## Persistent Storage

Each Cassandra pod received its own persistent volume through StatefulSet volume claim templates.

The storage layer was backed by a StorageClass so persistent volumes could be created automatically for each node.

The key requirement was that data should remain associated with the node even when pods were recreated.

## Service Discovery

A Headless Service was used for Cassandra.

This allowed direct DNS resolution of individual nodes, which was important for:

- cluster discovery
- intra-ring communication
- replication
- synchronization

## Scheduling

Node Affinity was used to associate Cassandra nodes with the intended placement strategy.

Pod Anti-Affinity reduced the chance of placing equivalent Cassandra nodes on the same Worker.

This improved failure-domain distribution.

## Automated Bootstrap

A post-install bootstrap mechanism was integrated into the Helm deployment.

It automated operations such as:

- creating required keyspaces/data spaces
- executing schema initialization
- loading initial data
- creating application users
- removing the default Cassandra user

The goal was to make cluster initialization reproducible rather than dependent on manual post-deployment steps.

## Key Lesson

Stateful migrations require more than persistent disks.

Identity, discovery, topology, placement and initialization must all be represented explicitly.
