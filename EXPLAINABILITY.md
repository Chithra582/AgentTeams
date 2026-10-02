# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **AgentTeams Collaborative Multi-Agent Platform** (`agentteams`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** AgentTeams Collaborative Multi-Agent Platform (`agentteams`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Collaborative Multi-Agent Runtime & Kubernetes Control Plane  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

AgentTeams is an enterprise-grade multi-agent runtime and control plane designed to orchestrate cooperative agent teams in audited, controlled environments. Operating on a robust Manager-Workers architecture, the platform enables diverse agent containers (QwenPaw, OpenClaw, Hermes, DeepSeek Harness) to collaborate within transparent Matrix protocol rooms alongside human supervisors.

### 1. Decision Architecture

The multi-agent orchestration, CRD lifecycle reconciliation, and gateway traffic management pipeline operates across a deterministic, five-stage architecture:

```
Developer / Cluster Task Event (Team Goal Specification / Task Dispatch / CRD Mutation)
    │
    ▼
[Stage 1: Intent & Team Goal Ingestion]
    │  - Decomposes high-level objectives into modular worker tasks
    │  - Establishes communication protocols and worker role allocations
    │  - Pre-allocates token limits and task dependency graphs
    ▼
[Stage 2: Worker Allocation & CRD Reconciliation]
    │  - Evaluates worker container requirements and maps custom CRDs
    │  - Provisions isolated Pod runtimes (Hermes, QwenPaw, OpenClaw)
    │  - Verifies resource limits against cluster quota constraints
    ▼
[Stage 3: Matrix Room Initialization & Identity Binding]
    │  - Creates secure room with cryptographic authorization tokens
    │  - Invites Manager, Worker agents, and Human Supervisor
    │  - Establishes immutable event logging stream for all agent communications
    ▼
[Stage 4: Shared Storage & Gateway Mediation]
    │  - Mediates all external inference traffic via Higress AI gateway
    │  - Intercepts and scrubs API keys and infrastructure credentials
    │  - Offloads large artifacts (>100KB) to MinIO presigned object storage
    ▼
[Stage 5: Consensus Verification & Audit Delivery]
    │  - Validates worker task deliverables against defined completion criteria
    │  - Evaluates security constraints and static code safety rules
    │  - Commits finalized outputs and archives complete session trajectory
    ▼
Validated Collaborative Multi-Agent Deliverable & Immutable Matrix Audit Trail
```

### 2. Decision Logic & Routing Formulations

AgentTeams evaluates worker allocation, task scheduling, and token consumption using deterministic mathematical models:

1. **Worker Allocation Affinity ($A_{\text{worker}}$)**:
   $$A_{\text{worker}} = (w_r \cdot R_{\text{reasoning}}) + (w_c \cdot C_{\text{coding}}) + (w_m \cdot M_{\text{memory}})$$
   where:
   - $R_{\text{reasoning}} \in [0, 1]$ represents problem decomposition difficulty.
   - $C_{\text{coding}} \in [0, 1]$ represents code generation and test execution intensity.
   - $M_{\text{memory}} \in [0, 1]$ represents context size demand.
   - Weights: $w_r = 0.40, w_c = 0.40, w_m = 0.20$ ($\sum w_i = 1.0$).

2. **Storage Token Savings Ratio ($S_{\text{token}}$)**:
   $$S_{\text{token}} = 1 - \frac{\text{Bytes}(\text{MinIO Presigned URI})}{\text{Bytes}(\text{Raw Payload File})}$$
   Enforced whenever raw artifact sizes exceed 100 KB to preserve active LLM context windows.

### 3. Thresholding & Refusal Decision Criteria

AgentTeams enforces strict integrity and operational boundaries:
- **Refusal to Operate Without Human Presence**: Agent collaboration rooms cannot begin autonomous task loops unless a designated Human Supervisor has joined the room (`ERR_HUMAN_SUPERVISOR_REQUIRED`).
- **Refusal of Direct Credential Ingestion**: Requests to pass raw API secrets, cluster tokens, or SSH keys directly to worker containers are rejected at the gateway layer (`ERR_DIRECT_CREDENTIAL_INJECTION_REFUSED`).
- **Turn Ceiling Enforcement**: Multi-agent conversational turns are capped at `max_turns: 25` per subtask to prevent circular debate loops (`WARN_TURN_BUDGET_REACHED`).
- **Cluster Boundary Guard**: Pod containers cannot execute privileged host-level operations or escape assigned Kubernetes namespaces (`ERR_NAMESPACE_ESCAPE_PROHIBITED`).

### 4. Fallback Decision Mechanism

The platform maintains reliable execution through multi-tier fault recovery:
- **Gateway Provider Failover**: If the primary foundation model endpoint experiences rate limits or network degradation, Higress dynamically routes traffic to designated enterprise fallback providers.
- **Worker Self-Healing**: Failed worker pods are automatically rescheduled according to Kubernetes restart policies, with task states recovered from MinIO snapshots.
- **Graceful Token Throttling**: When room conversation sizes approach context limits, the Manager agent triggers deterministic message summarization to condense history before dispatching new turns.

### 5. Human-in-the-Loop Governance

Human authority is maintained at every level of the collaboration hierarchy:
- **Supervisor Veto Authority**: The human participant retains immediate override and veto power over any worker proposal or planned file modification.
- **Emergency Room Eviction**: The human supervisor can execute the `/abort` or `/kick` room command to terminate worker execution loops instantly.
- **Mandatory Approval Gates**: Code merges, infrastructure deployments, and external service mutations require explicit human sign-off before execution.

---

## The Data It Uses

AgentTeams operates under strict data privacy, least-privilege access, and enterprise governance standards.

### 1. Ingested Input Data

The agent processes only operational data required for task coordination:
- **Task Directives**: User requests, goal specifications, and project issue descriptions.
- **Matrix Room Messages**: Structured JSON events exchanged between participating agents and human supervisors.
- **Kubernetes Manifests**: Declarative Custom Resource Definitions (CRDs) specifying worker configurations and team topologies.
- **Source Code & Documentation**: Project files ingested from mounted workspace volumes.

### 2. Configuration & Reference Data

- **Worker Runtime Catalogs**: Pre-built container images with embedded tooling (Hermes, QwenPaw, OpenClaw, DeepSeek Harness).
- **Cluster Resource Quotas**: Namespace CPU, memory, and storage allocations.
- **Higress Gateway Route Tables**: Upstream LLM endpoints, token limits, and rate-limiting policies.

### 3. Base Model & Inference Lineage

- **Deterministic Control Plane Engines**: Kubernetes controllers, Matrix client bridges, and CRD reconcilers executed natively in Go and Python (100% deterministic with zero LLM variance).
- **Foundation LLMs**: Enterprise-hosted or API-gated models (`claude-3-5-sonnet`, `gpt-4o`, `gemini-2.0-flash`, `deepseek-coder`) utilized for subtask reasoning and code generation.
- **Zero Training on Proprietary Data**: Task code, internal Matrix room transcripts, and infrastructure configurations are never used to train foundation models.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Compliance**: Hardened against prompt injection, insecure plugin design, and supply chain attacks.
- **Epistemic Pod Isolation**: Worker containers operate within isolated network namespaces without cross-container volume access.
- **Encrypted Local Storage**: Artifacts stored in MinIO and conversation logs in Matrix are encrypted at rest and in transit via TLS.
- **Zero Third-Party Telemetry**: Telemetry and metrics remain within the self-hosted Kubernetes cluster environment.

---

## Limitations

Understanding the operational boundaries and technical constraints of AgentTeams is essential for effective deployment.

### 1. Matrix Room Event Desynchronization Under Heavy Load
- **Limitation**: When dozens of concurrent workers broadcast messages simultaneously, event delivery ordering can experience transient lag.
- **Mitigation**: The Manager agent enforces sequence IDs and turn-taking tokens to maintain strict causal ordering across worker contributions.

### 2. Kubernetes Cluster Quota Starvation
- **Limitation**: Spawning multiple multi-agent teams simultaneously can exhaust available cluster CPU and GPU resources.
- **Mitigation**: The CRD controller queues pending worker requests and enforces cluster priority tiers until resources are released.

### 3. MinIO Storage Synchronization Latency
- **Limitation**: Network throughput constraints during large file transfers (>500MB) can delay worker synchronization.
- **Mitigation**: Presigned URLs support chunked multipart uploads, and agents exchange compressed differential patches rather than full files.

### 4. Cross-Worker Semantic Misunderstanding
- **Limitation**: Workers running different model sizes may occasionally misunderstand specialized instruction nuances from other workers.
- **Mitigation**: Communication protocols enforce structured JSON schemas with explicit input/output contracts for all inter-worker handoffs.

### 5. Multi-Container Cold Start Latency
- **Limitation**: Pulling large worker container images on new Kubernetes nodes introduces initial provisioning delays.
- **Mitigation**: The platform uses image pre-pulling daemons and lightweight container bases to minimize startup times.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested task specifications, Matrix events & CRDs | Section 1 | Verified |
| - Configuration, worker catalogs & route tables | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Matrix room event desynchronization under load | Section 1 | Verified |
| - Kubernetes cluster quota starvation | Section 2 | Verified |
| - MinIO storage synchronization latency | Section 3 | Verified |
| - Cross-worker semantic misunderstanding | Section 4 | Verified |
| - Multi-container cold start latency | Section 5 | Verified |
