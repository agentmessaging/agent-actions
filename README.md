# Agent Actions Protocol (AAP)

**Open Protocol for Structured UI-to-Agent Interactions** - When AI agents render HTML interfaces, users interact with them. AAP captures those interactions as structured JSON records, stores them immutably, and notifies the agent.

[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Spec Version](https://img.shields.io/badge/spec-v1.0-brightgreen.svg)](spec/)
[![Website](https://img.shields.io/badge/website-agentactions.org-10b981)](https://agentactions.org)

## The Problem

AI agents increasingly render rich HTML interfaces — dashboards, forms, reports, interactive tools. Users interact with these UIs, but the interactions have no standard way to flow back to the agent.

| Current Approach | Problem |
|-----------------|---------|
| **console.log scraping** | Fragile, no structure, lost on refresh |
| **Polling the DOM** | Complex, race conditions, not sandboxed |
| **Custom WebSocket per page** | Over-engineered, security nightmare |

## The Solution

AAP defines a minimal protocol: one bridge script, one message format, one storage pattern.

```
Agent writes HTML  -->  Dashboard renders in iframe  -->  User clicks button
                                                              |
Agent reads JSON   <--  File written + notification   <--  postMessage to parent
                                                              |
                                                         HTTP POST to API
```

## Quick Example

**In the agent's canvas HTML:**

```html
<button onclick="maestro.send('click', 'approve-btn', { approved: true })">
  Approve
</button>
```

**What the agent receives (JSON file):**

```json
{
  "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "timestamp": "2026-05-18T15:30:00.000Z",
  "canvasFile": "reports/dashboard.html",
  "action": "click",
  "element": "approve-btn",
  "data": { "approved": true },
  "summary": "User click 'approve-btn' on reports/dashboard.html with data: {approved: true}"
}
```

## Standard Action Vocabulary

| Action | Use case |
|--------|----------|
| `click` | Button press, link activation |
| `submit` | Form submission |
| `change` | Input value changed |
| `select` | Option selected from dropdown/list |
| `toggle` | Boolean switch toggled |
| `dismiss` | Modal/notification dismissed |
| `navigate` | In-canvas navigation |
| `custom` | Application-specific action |

Custom actions beyond this vocabulary are allowed — the `action` field is freeform.

## Specification

The full protocol specification is in the [`spec/`](spec/) directory:

- [01-introduction.md](spec/01-introduction.md) — Design principles and overview
- [02-bridge.md](spec/02-bridge.md) — Bridge script injection and API
- [03-messages.md](spec/03-messages.md) — Action message format
- [04-storage.md](spec/04-storage.md) — Interaction record format and file storage
- [05-transport.md](spec/05-transport.md) — Transport and API endpoints
- [06-security.md](spec/06-security.md) — Security considerations
- [07-implementers.md](spec/07-implementers.md) — Implementer checklist

Or read the complete spec in one file: [PROTOCOL.md](PROTOCOL.md)

## Relationship to AMP & AID

AAP is **independent** — it does not require AMP or AID. All three are complementary:

| Protocol | Direction | Purpose |
|----------|-----------|---------|
| **AAP** | User → Agent | Structured UI interactions |
| **[AMP](https://agentmessaging.org)** | Agent → Agent | Signed messages and routing |
| **[AID](https://agentids.org)** | Agent identity | Cryptographic authentication |

All three share the same agent directory structure (`~/.aimaestro/agents/<id>/`).

## Claude Code Plugin

This repo includes a **canvas-actions** skill for Claude Code agents. When installed, agents automatically know how to create canvas pages, embed interactive data, and process user interactions.

### Installation

The skill is distributed via the [AI Maestro plugin](https://github.com/23blocks-OS/ai-maestro-plugins):

```bash
# Install AI Maestro plugin (includes AAP + AMP + AID skills)
git clone https://github.com/23blocks-OS/ai-maestro-plugins.git
cd ai-maestro-plugins
./build-plugin.sh --clean
./install-plugin.sh -y
```

Or add AAP as a source in your own plugin manifest:

```json
{
  "name": "agent-actions",
  "type": "git",
  "repo": "https://github.com/agentmessaging/agent-actions.git",
  "ref": "main",
  "map": {
    "skills/canvas-actions": "skills/canvas-actions"
  }
}
```

### What the Skill Teaches Agents

- **When** to create a canvas (proactive triggers like "show me", "visualize", "dashboard for")
- **How** to write data-driven interactive HTML with embedded JSON
- **Where** to store canvas files (`~/.aimaestro/agents/<id>/canvas/`)
- **How** to process `[CANVAS]` notifications and read interaction records
- **Best practices** for the `maestro.send()` bridge API

## Reference Implementation

[AI Maestro](https://github.com/23blocks-OS/ai-maestro) is the reference implementation of AAP. It includes the bridge script injection, canvas rendering, interaction storage, and agent notification.

## Roadmap

| Version | Scope |
|---------|-------|
| **v1.0** (current) | One-way canvas interactions (user → agent) |
| **v1.1** | Bidirectional — agents push updates back to canvas |
| **v1.2** | Interaction acknowledgment — agent confirms receipt |
| **v2.0** | Agent-defined UI components (widgets, forms, controls) |

## License

This specification is released under the [MIT License](LICENSE).

Copyright 2026 Juan Pelaez / 23blocks.
