---
name: matrix-im-room-mediator
description: "Bridges multi-runtime communication across Matrix protocol rooms with human auditability."
---

# Matrix IM Room Mediator

## Overview
The `matrix-im-room-mediator` skill establishes secure, auditable communication channels between human coordinators, Manager agents, and Worker agents using the decentralized Matrix messaging protocol.

## Protocol Mechanics
- **Room Visibility:** Every collaboration room includes human operators, ensuring zero black-box autonomy.
- **Message Schemas:** Messages between agents are formatted with structured event headers, role tags, and execution status.
- **Intervention Hooks:** Human participants can type directives directly into the Matrix room to steer or pause worker tasks.
