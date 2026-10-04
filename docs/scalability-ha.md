🌐 **Language:** **English** | [Français](fr/scalability-ha.md)

# Scalability & High Availability

## Replicated Application Workloads

Stateless application workloads were deployed with multiple replicas.

Kubernetes could recreate failed pods and maintain availability without depending on a single application instance.

## Health Probes

### Startup Probe

Used for applications with longer initialization times to avoid premature restarts.

### Readiness Probe

Controlled whether a pod was ready to receive traffic.

### Liveness Probe

Detected unhealthy or blocked application instances so Kubernetes could restart them.

## Horizontal Pod Autoscaler

HPA was used for the application layer.

Scaling considered CPU and memory utilization and adjusted replica counts according to load.

## Cassandra Availability

Cassandra used StatefulSets because each node required stable identity and persistent state.

Availability-related mechanisms included:

- multiple logical Data Centers
- persistent volumes
- Node Affinity
- Pod Anti-Affinity
- Headless Services
- automated initialization

## Failure-Domain Awareness

Cassandra nodes were distributed across worker nodes where possible to reduce the impact of a single Worker failure.

## High-Availability Principle

High availability was treated as a combination of:

- workload redundancy
- health awareness
- automatic recreation
- readiness-aware routing
- horizontal scaling
- persistent-state design
- topology-aware scheduling
- monitoring
