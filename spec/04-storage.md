# 4. Interaction Record Format & Storage

## Interaction Record

The stored interaction (server-side):

```json
{
  "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "timestamp": "2026-05-18T15:30:00.000Z",
  "canvasFile": "reports/dashboard.html",
  "action": "submit",
  "element": "approve-button",
  "data": { "comments": "Looks good" },
  "summary": "User submit 'approve-button' on reports/dashboard.html with data: {comments: Looks good}"
}
```

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | UUID | Yes | Unique interaction identifier |
| `timestamp` | ISO 8601 | Yes | When the interaction occurred |
| `canvasFile` | string | Yes | Relative path of the canvas HTML file |
| `action` | string | Yes | The action verb |
| `element` | string | No | Element identifier |
| `data` | object | No | Arbitrary payload |
| `summary` | string | Yes | Human-readable summary for the agent |

## File Storage

- **Path**: `~/.aimaestro/agents/<agentId>/canvas/interactions/`
- **Filename**: `<ISO-timestamp>-<UUID>.json` (with `:` and `.` replaced by `-`)
- **One file per interaction** (append-only, immutable)
- **Sorted by filename** = sorted by time

Example filename:
```
2026-05-18T15-30-00-000Z-a1b2c3d4-e5f6-7890-abcd-ef1234567890.json
```
