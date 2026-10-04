🌐 **Language:** **English** | [Français](fr/architecture.md)

# Architecture

## Legacy Context

The starting point was a VM-based platform composed of Java/Spring applications hosted on Tomcat and a distributed Cassandra data layer.

Operations depended heavily on server-level configuration and manual procedures for deployment, scaling, initialization and recovery.

## Validation Environment

The documented validation lab used Kubernetes installed with kubeadm on virtual machines:

| Node | CPU | RAM | Role |
|---|---:|---:|---|
| Control Plane | 3 vCPU | 6 GB | Cluster management |
| Worker 1 | 3 vCPU | 6 GB | Workloads |
| Worker 2 | 3 vCPU | 6 GB | Workloads |
| Worker 3 | 3 vCPU | 6 GB | Workloads |

The nodes ran Ubuntu Server 22.04 LTS and Kubernetes v1.33.

Calico provided pod networking and supported NetworkPolicies.

## Workload Design

### Application Layer

Java/Tomcat applications were deployed as replicated Kubernetes Deployments.

Configuration was externalized with ConfigMaps, and internal access was provided through ClusterIP Services.

### Data Layer

Cassandra was deployed using StatefulSets because stable identity and persistent storage were required.

The design also included Headless Services, topology-aware scheduling and automated bootstrap.

## Network Architecture

External traffic followed this path:

```text
Users
  ↓
External Reverse Proxy
  ↓
NGINX Ingress Controller
  ↓
ClusterIP Service
  ↓
Application Pods
```

Cassandra used a Headless Service so individual nodes could be resolved directly through DNS for discovery, replication and intra-ring communication.

A dedicated network interface was also used for cluster-related traffic in the lab environment.

## Scheduling

Node Affinity and Pod Anti-Affinity were used for Cassandra placement to distribute stateful nodes across workers and reduce failure concentration.

## Design Principle

The central design principle was to replace host-specific operational procedures with declarative platform behavior wherever practical.
