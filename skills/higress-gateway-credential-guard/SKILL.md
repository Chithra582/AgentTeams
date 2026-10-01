---
name: higress-gateway-credential-guard
description: "Sanitizes egress AI model traffic and prevents raw API key exposure across worker runtimes."
---

# Higress Gateway Credential Guard

## Overview
The `higress-gateway-credential-guard` skill routes all model inference requests through a centralized Higress AI Gateway, isolating enterprise credentials from untrusted agent code.

## Security Principles
1. **Consumer Token Isolation:** Worker containers operate solely with ephemeral consumer tokens; raw API keys and GitHub tokens never enter the container.
2. **Egress Inspection:** Gateway enforces rate limits, token quotas, and content safety filters on all outgoing traffic.
3. **Audit Trails:** Centralized logging of all model invocations, prompt token counts, and completion latencies.
