# Challenges & Lessons Learned

## 1. Migrating Legacy Assumptions

Legacy applications often assume:

- stable hosts
- local configuration
- fixed ports
- long startup times
- direct server access

These assumptions need to be identified before containerization.

## 2. Stateful Systems Need Different Thinking

Stateful workloads cannot be treated like ordinary stateless replicas.

Stable identity, storage, bootstrap behavior and service discovery become central design concerns.

## 3. Readiness Matters as Much as Liveness

A process can be alive while the application is still unable to serve traffic.

Using readiness checks correctly helps prevent traffic from reaching workloads too early.

## 4. GitOps Reduces Hidden Changes

Manual cluster changes create drift.

Keeping deployment intent in Git improves traceability and makes it easier to understand why a given state exists.

## 5. Monitoring Must Be Designed Early

It is easier to operate a platform when metrics, dashboards and alerts are considered during the architecture phase instead of being added after failures occur.

## 6. Security Is Cross-Cutting

Security decisions affect CI, registries, Kubernetes identities, secrets, networking and runtime behavior.

## 7. Migration Is More Than Containerization

The most important lesson was that moving to Kubernetes is not simply converting applications into containers.

A successful migration also changes:

- delivery workflows
- configuration management
- observability
- scaling
- failure recovery
- security boundaries
- operational ownership
