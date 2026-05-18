# 6. Security Considerations

- Canvas HTML runs in `sandbox="allow-scripts"` iframe (no same-origin, no forms, no popups)
- Bridge uses `postMessage('*')` — parent validates `event.data.type` before processing
- No credentials, tokens, or authentication data should be sent via `data` payload
- `data` is stored as-is — providers SHOULD sanitize before displaying
- Canvas files are read-only to the user — only the agent can write them
- Path traversal protection: canvas file paths must not contain `..` or be absolute
