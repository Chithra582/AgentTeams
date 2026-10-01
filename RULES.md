# RULES — Operational Invariants for AgentTeams Collaborative Multi-Agent Platform

1. **Mandatory Human Room Presence:** All newly provisioned collaboration rooms must include designated human coordinator accounts with administrative join privileges.
2. **Credential Gateway Invariant:** Workers must never be supplied with direct upstream API keys. All inference traffic must route through the Higress AI Gateway.
3. **Storage Reference Passing:** Files exceeding 100 KB must be staged into MinIO shared storage and referenced by URI rather than serialized into context windows.
4. **Declarative Reconcile Loop:** Modifications to agent state, skills, or environment variables must be applied through Kubernetes CRD specs.
5. **Human Approval Gate:** Require explicit human confirmation prior to spinning down Worker Pods, deleting shared storage buckets, or mutating production routes.
