# Architecture

## Legacy Context

The starting point was a VM-based enterprise platform composed of Java applications hosted on Tomcat and a distributed Cassandra data layer.

The architecture had grown around traditional server operations, which made deployment, scaling and recovery more dependent on host-level procedures.

## Migration Objectives

The target design aimed to:

- standardize application deployment
- improve workload isolation
- introduce declarative infrastructure behavior
- separate application build from deployment state
- improve observability
- support horizontal scaling where appropriate
- reduce configuration drift
- improve service recovery

## Kubernetes Design

The application layer was modeled as Kubernetes workloads and exposed through Kubernetes networking primitives.

The design separated:

- stateless application workloads
- stateful database workloads
- configuration
- secrets
- persistent data
- ingress
- monitoring

### Stateless Workloads

Java/Tomcat application components were deployed as replicated workloads so that multiple instances could run concurrently.

This enabled:

- rolling updates
- replica-based availability
- horizontal scaling
- readiness-aware traffic routing

### Stateful Workloads

The Cassandra layer required stateful workload orchestration.

Important concerns included:

- stable network identity
- persistent storage
- controlled startup behavior
- cluster membership
- service discovery

### Networking

Kubernetes Services provided stable service discovery inside the cluster.

Ingress was used for controlled external HTTP access, while reverse-proxy behavior was kept separate from application containers.

## Design Principle

The core principle was to replace host-specific operational knowledge with declarative platform behavior wherever possible.

Instead of asking:

> Which server should this application run on?

the platform could answer:

> What state should this workload have, and how should Kubernetes maintain that state?
