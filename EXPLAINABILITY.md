# EXPLAINABILITY — AgentTeams Collaborative Multi-Agent Platform

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* AgentTeams Collaborative Multi-Agent Platform (`agent-teams-controller`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Collaborative Multi-Agent Runtime & Kubernetes Control Plane  

---

## 1. Overview & Operational Purpose

The **AgentTeams Collaborative Multi-Agent Platform** (`agent-teams-controller`) is an enterprise-grade multi-agent runtime and control plane designed to orchestrate cooperative agent teams in audited, controlled environments. Operating on a robust Manager-Workers architecture, the platform enables diverse agent containers (QwenPaw, OpenClaw, Hermes, DeepSeek Harness) to collaborate within transparent Matrix protocol rooms alongside human supervisors.

By unifying declarative Kubernetes custom resources, centralized Higress AI gateway traffic isolation, and token-saving MinIO shared storage, AgentTeams delivers high-throughput enterprise multi-agent workflows with zero credential leakage and end-to-end auditability.

---

## 2. How the Agent Decides (Decision-Making Logic)

AgentTeams Collaborative Multi-Agent Platform operates across a deterministic, multi-stage decision pipeline:

```
[Stage 1: Intent & Team Goal Ingestion] ──> [Stage 2: Worker Allocation & Topology] ──> [Stage 3: Matrix Room Initialization]
                                                                                                        │
                                                                                                        ▼
[Stage 6: Artifact & Audit Trail Archive] <── [Stage 5: Gateway Credential Mediation] <── [Stage 4: Shared Storage & Execution]
```

### 2.1 Intent Ingestion & Goal Parsing
- **Decision:** Decomposes complex user requests into discrete, parallelizable sub-tasks with well-defined input/output contracts.
- **Rules:** If a task requires specialized coding or research capabilities, assign dedicated worker roles rather than overloading a single agent.

### 2.2 Worker Allocation & CRD Reconciliation
- **Decision:** Selects optimal runtime images (e.g., Hermes for coding, QwenPaw for general reasoning) and reconciles Kubernetes Worker CRDs.
- **Rules:** Ensure worker container resource limits (CPU/memory) adhere to cluster quotas. Hot-mount assigned skills dynamically.

### 2.3 Matrix Room & Communication Setup
- **Decision:** Initializes a dedicated Matrix collaboration room, issuing cryptographic invite tokens to the Manager, Workers, and Human Supervisor.
- **Rules:** A room cannot begin autonomous operation without confirming human coordinator presence and write permissions.

### 2.4 Gateway Mediation & Shared Artifacts
- **Decision:** Routes model inference via the Higress gateway while offloading intermediate documents to MinIO storage.
- **Rules:** Strip API credentials from worker payloads. Enforce presigned URI exchanges for files exceeding 100 KB to conserve token budgets.

---

## 3. Data & Privacy

| Category | Policy / Handling |
|---|---|
| **Input Data** | In-memory evaluation of user directives, Matrix message events, and declarative Kubernetes manifests. |
| **Output Artifacts** | Kubernetes YAML specifications, Matrix room conversation records, and shared MinIO files. |
| **Telemetry & Logging** | Local deterministic console logging and Matrix room history; zero data scraping to external vendors. |
| **Third-Party APIs** | Model requests routed through self-hosted Higress AI Gateway to enterprise-approved LLM endpoints. |

AgentTeams Collaborative Multi-Agent Platform complies with operational security and privacy standards:
- **No Cloud Data Exfiltration:** Operates within private Kubernetes clusters and self-hosted Matrix infrastructure with zero external telemetry transmission.
- **Epistemic Isolation:** Worker containers run in isolated Pod boundaries; inter-agent memory is isolated to explicit Matrix room events.
- **Sanitized Model Payloads:** Sensitive infrastructure credentials, cluster secrets, and GitHub PATs are intercepted and held at the gateway layer.
- **Data Minimization:** Workers only receive the specific task instructions and object pointers necessary to execute their assigned role.

---

## 4. Known Limitations & Failure Modes

Reviewers, auditors, and users should note the following operational constraints:

1. **Heterogeneous Worker Cold Starts**
   - *Limitation:* Spinning up complex containerized worker runtimes (e.g., heavy coding environments) on demand introduces initial container pulling latency.
   - *Mitigation:* The controller supports pre-warmed worker pools and lightweight QwenPaw runtimes to achieve sub-second readiness.

2. **Matrix Message Serialization Rate Limits**
   - *Limitation:* Extreme conversational velocity across dozens of concurrent workers can generate high event volumes in a single Matrix room.
   - *Mitigation:* The platform batches progress notifications and pushes large logs to MinIO, posting summary links into the room.

3. **Kubernetes API Server Throttling**
   - *Limitation:* Rapid burst reconciliation of hundreds of custom resources can trigger API server rate limiting in large clusters.
   - *Mitigation:* The controller employs client-go rate limiters, informers with exponential backoff, and local caching.

4. **Multi-Runtime Tool Schema Divergence**
   - *Limitation:* Differing worker runtimes (OpenClaw vs. Hermes) may define tool call structures with subtle schema discrepancies.
   - *Mitigation:* The platform standardizes tool invocations using the Model Context Protocol (MCP) specification across all worker flavors.

---

## 5. Verification, Safety & Human Oversight

The agent implements comprehensive oversight mechanisms:
- **Real-Time Human Approval Gate:** Mandatory explicit operator confirmation is required prior to applying disruptive cluster changes, modifying network ingress policies, or publishing external deliverables.
- **Emergency Session Interrupt:** Human supervisors present in the Matrix room can pause or terminate any worker process immediately via standard `/stop` commands.
- **Step Quota Guardrails:** Autonomous task iterations are capped by configurable step thresholds (default: 20 turns) to prevent runaway worker processes.
- **Structured Audit Logging:** Every Matrix message, CRD state transition, and gateway model call is preserved in tamper-evident logs for post-incident review.
