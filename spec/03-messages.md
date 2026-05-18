# 3. Action Message Format

The `postMessage` payload sent from iframe to parent:

```json
{
  "type": "canvas:interaction",
  "action": "submit",
  "element": "approve-button",
  "data": { "comments": "Looks good", "rating": 5 }
}
```

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | Always `"canvas:interaction"` |
| `action` | string | Yes | Action verb (see Standard Actions) |
| `element` | string | No | Element identifier (id, name, label) |
| `data` | object | No | Arbitrary key-value payload |

## Standard Action Vocabulary

| Action | Use case |
|--------|----------|
| `click` | Button press, link activation |
| `submit` | Form submission |
| `change` | Input value changed |
| `select` | Option selected from dropdown/list |
| `toggle` | Boolean switch toggled |
| `dismiss` | Modal/notification dismissed |
| `navigate` | In-canvas navigation (tab switch, page change) |
| `custom` | Application-specific action (use `data` for details) |

Custom actions beyond this vocabulary are allowed — the `action` field is freeform.
