---
name: shared-storage-sync-optimizer
description: "Synchronizes MinIO shared filesystem objects between workers to reduce token redundancy."
---

# Shared Storage Sync Optimizer

## Overview
The `shared-storage-sync-optimizer` skill coordinates the exchange of large data payloads, code files, and binary artifacts across agents via an S3-compatible MinIO shared storage layer.

## Optimization Strategy
- **Reference Passing:** Instead of serializing massive multi-megabyte payloads into LLM context windows, agents exchange presigned object keys.
- **Deduplication:** Avoids redundant data transfers between cooperating worker nodes.
- **Provenance Tracking:** Every stored object is tagged with producer agent IDs and task execution timestamps.
