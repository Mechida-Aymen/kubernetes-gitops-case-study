# Scalability & High Availability

## Application Availability

Stateless application workloads were deployed with multiple replicas to reduce dependence on a single instance.

Kubernetes could replace failed pods automatically and keep traffic away from workloads that were not ready.

## Health Probes

Different probe types were used for different purposes:

### Startup Probe

Protected slow-starting applications from being restarted before initialization had completed.

### Readiness Probe

Controlled whether a workload was eligible to receive traffic.

### Liveness Probe

Detected workloads that were running but no longer healthy.

## Horizontal Scaling

Horizontal Pod Autoscaling was used for suitable stateless workloads.

Scaling decisions could be based on resource utilization while enforcing minimum and maximum replica limits.

## Stateful Availability

The database layer required a different availability strategy because stateful systems depend on stable identity, persistent data and cluster membership.

## High-Availability Principle

High availability was treated as a combination of:

- redundancy
- health awareness
- automatic recovery
- traffic control
- persistent-state design
- monitoring
