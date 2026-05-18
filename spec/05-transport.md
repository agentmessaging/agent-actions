# 5. Transport & API Endpoints

## Transport Flow

```
Canvas iframe
  | window.parent.postMessage()
  v
Parent window (Dashboard)
  | POST /api/agents/:id/canvas/interactions
  v
Provider API
  | Write JSON file + tmux notification
  v
Agent
```

## API Endpoints

### Submit Interaction

```
POST /api/agents/:id/canvas/interactions
Content-Type: application/json

{
  "action": "submit",
  "element": "approve-button",
  "canvasFile": "reports/dashboard.html",
  "data": { "comments": "Looks good" }
}
```

**Responses:**

```
201 Created
{ "id": "a1b2c3d4-...", "summary": "User submit 'approve-button' on reports/dashboard.html" }

400 Bad Request
{ "error": "missing_field", "message": "action is required" }

404 Not Found
{ "error": "not_found", "message": "Agent 'xyz' not found" }
```

### List Interactions

```
GET /api/agents/:id/canvas/interactions?limit=50
```

**Response:**

```json
{
  "interactions": [
    {
      "id": "a1b2c3d4-...",
      "timestamp": "2026-05-18T15:30:00.000Z",
      "action": "submit",
      "element": "approve-button",
      "canvasFile": "reports/dashboard.html",
      "data": { "comments": "Looks good" },
      "summary": "User submit 'approve-button' on reports/dashboard.html with data: {comments: Looks good}"
    }
  ]
}
```

## Agent Notification

When an interaction is stored, the provider SHOULD notify the agent:

```
[CANVAS] reports/dashboard.html: User submit 'approve-button' on reports/dashboard.html with data: {comments: Looks good}
```

Notification is fire-and-forget. Failure to notify does not affect interaction storage.
