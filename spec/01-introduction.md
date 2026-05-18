# 1. Introduction

AI agents increasingly render rich HTML interfaces — dashboards, forms, reports, interactive tools. Users interact with these UIs, but the interactions have no standard way to flow back to the agent. AAP solves this with a minimal, open protocol.

## Design Principles

- **Minimal** — One bridge script, one message format, one storage pattern
- **Structured** — Every interaction is a typed JSON record, not raw DOM events
- **Immutable** — Interactions are append-only files, never modified
- **Agent-notified** — The agent is told when something happens, in real time

## Terminology

| Term | Definition |
|------|-----------|
| **Canvas** | Agent-authored HTML rendered in a sandboxed iframe |
| **Action** | A discrete user interaction (click, submit, change, etc.) |
| **Element** | Identifier of the UI element acted upon |
| **Interaction** | The complete record of an action (action + element + data + metadata) |
| **Bridge** | JavaScript injected into canvas HTML that provides `maestro.send()` |
| **Provider** | The system that receives, stores, and delivers interactions (e.g., AI Maestro) |
