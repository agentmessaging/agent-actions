# 7. Implementer Checklist

For building an AAP-compatible provider:

- [ ] Inject bridge script into canvas HTML before rendering
- [ ] Listen for `postMessage` with `type: 'canvas:interaction'`
- [ ] Validate `action` field is present
- [ ] Generate UUID and ISO timestamp
- [ ] Build human-readable summary
- [ ] Store interaction as JSON (recommended: file-per-interaction)
- [ ] Notify agent (optional, provider-specific mechanism)
- [ ] Expose API for listing interactions (optional)

## Relationship to AMP & AID

- **AAP is independent** — does not require AMP or AID
- **Complementary**: AAP handles user-to-agent UI actions; AMP handles agent-to-agent messaging; AID handles agent authentication
- All three use the same agent directory structure (`~/.aimaestro/agents/<id>/`)
- AAP interactions can trigger AMP messages (e.g., "user approved the report" -> send notification to another agent)
