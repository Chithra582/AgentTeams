---
name: k8s-crd-lifecycle-controller
description: "Manages Kubernetes custom resources (Worker, Team, Human) and Helm deployments."
---

# Kubernetes CRD Lifecycle Controller

## Overview
The `k8s-crd-lifecycle-controller` skill manages declarative Kubernetes Custom Resource Definitions (CRDs) including `Worker`, `Team`, and `Human` resources for elastic agent infrastructure.

## CRD Resource Management
- **Declarative Reconciliation:** Continuous reconciliation loop ensuring running Pods match desired CR specifications.
- **Dynamic Skill Injection:** Mounts custom skills into running Worker containers without triggering Pod restarts.
- **Health Probing:** Monitored liveness and readiness probes to detect hung worker processes and auto-recover state.
