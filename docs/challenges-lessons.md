🌐 **Language:** **English** | [Français](fr/challenges-lessons.md)

# Challenges & Lessons Learned

## 1. Legacy Applications Carry Host Assumptions

The migration required identifying assumptions around host-local configuration, fixed network expectations, deployment paths and startup behavior.

Containerization alone does not remove these assumptions.

## 2. Kubernetes Networking Required Careful Design

Cluster traffic, ingress, Cassandra discovery and monitoring flows had different requirements.

Calico, ClusterIP Services, Headless Services, NGINX Ingress and the external reverse proxy each solved a different part of the problem.

## 3. Stateful Workloads Are Fundamentally Different

Cassandra required stable identity, persistent storage, controlled placement, topology awareness and automated bootstrap.

## 4. Dynamic Helm Generation Reduced Repetition

Generating Cassandra topology from Helm values reduced repetitive resource definitions and made logical Data Centers easier to manage.

## 5. Initialization Should Be Automated

Schema, initial data and user initialization were automated so deployments were consistent and reproducible.

## 6. Startup, Readiness and Liveness Solve Different Problems

These probes were used for different lifecycle stages and should not be treated as interchangeable checks.

## 7. GitOps Reduced Hidden Changes

Keeping deployment intent in Git improved traceability and made configuration drift visible.

## 8. External Secrets Improved Separation of Concerns

Vault remained outside the cluster while External Secrets Operator handled synchronization into Kubernetes.

## 9. Observability Needed Multiple Perspectives

Useful monitoring required infrastructure, Kubernetes-resource, container, Cassandra and JVM metrics together.

## 10. Migration Is More Than Containerization

The project changed delivery workflows, configuration management, networking, secrets, monitoring, scaling and recovery—not only packaging.
