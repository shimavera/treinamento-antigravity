---
name: evolution-api
description: Evolution API — open-source WhatsApp integration platform. Use when working with Evolution API instances, sending/receiving WhatsApp messages via Evolution API, configuring webhooks, managing instances (QR Code, Pairing Code), building n8n workflows with Evolution API, or migrating from Meta WhatsApp API to Evolution API. Covers Baileys (free) and Cloud API (official Meta) modes.
---

# Evolution API — Complete Reference

Open-source WhatsApp integration platform. Two connection modes:
- **Baileys** (free, no Meta approval, QR Code/Pairing Code)
- **Cloud API** (official Meta, templates required outside 24h window)

**Docs:** https://doc.evolution-api.com/v2/en/
**Repo:** https://github.com/EvolutionAPI/evolution-api

---

## Quick Reference

- [Instance Management](instances.md) — create, connect, QR code, pairing code
- [Sending Messages](messages.md) — text, media, buttons, lists, polls, reactions
- [Webhooks & Events](webhooks.md) — receive messages, 20 event types, payload structure
- [n8n Integration](n8n-integration.md) — community node, workflow patterns, expression mapping
- [Groups](groups.md) — create, manage participants, invite links
- [AI & Chatbot](ai-chatbot.md) — native integrations with OpenAI, Typebot, Dify, Flowise, n8n

## Authentication (Two-Tier)

| Level | Header | Scope |
|-------|--------|-------|
| **Global API Key** | `apikey: AUTHENTICATION_API_KEY` | All instances |
| **Instance Token** | `apikey: instance_hash` | Single instance only |

## Critical Differences: Meta API vs Evolution API

| Data | Meta API Expression | Evolution API Expression |
|------|-------------------|------------------------|
| Message text | `$json.body.entry[0].changes[0].value.messages[0].text.body` | `$json.data.message.conversation` |
| Sender number | `$json.body.entry[0].changes[0].value.messages[0].from` | `$json.data.key.remoteJid` |
| Contact name | `$json.body.entry[0].changes[0].value.contacts[0].profile.name` | `$json.data.pushName` |
| Message type | `$json.body.entry[0].changes[0].value.messages[0].type` | `$json.data.messageType` |
| Message ID | `$json.body.entry[0].changes[0].value.messages[0].id` | `$json.data.key.id` |
| Is from me? | N/A | `$json.data.key.fromMe` |
