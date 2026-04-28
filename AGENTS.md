This folder contains the source for a Skilled Agent originally built for the Valet runtime. Changes should follow the Skilled Agent open standard.

Transaction Watchdog reacts to Mercury webhooks. For every outgoing payment of $1,000 or more, it searches a Notion vendor database for the counterparty and either posts a quiet note to `#finance` (known vendor) or DMs the owner (unknown vendor). It does not move money, write to Notion, or call Mercury — the webhook payload is the only input.

## Setup

### Connectors

- **notion-mcp**: Read-only Notion access. The agent searches a Notion vendor database to decide whether the counterparty on a transaction is a known recurring vendor. Org-scoped by default — every agent in the org can reuse it.

### Channels

- **mercury-webhook** (webhook): Receives Mercury account events. The agent filters to transaction events of $1,000 or more in the outgoing direction; smaller amounts and incoming transfers are skipped. Org-scoped.
- **slack** (per-agent): Each agent that participates in Slack gets its own Slack bot. This bot posts summaries to `#finance` and DMs the account owner. The org-level Slack connection is a one-time prerequisite.

### Secrets

- **NOTION_TOKEN**: Notion internal integration token used by `notion-mcp`. Create at https://www.notion.so/profile/integrations. After creating, open the vendor database in Notion → "..." → Connections → add the integration. Without that share, search returns empty even with a valid token. Format: `secret_...`. Org-scoped.
- **MERCURY_WEBHOOK_SECRET**: Mercury's signing secret for webhook payloads. Configure the webhook in Mercury at Settings → Developers → Webhooks, pointing at the URL Valet generates when the channel is created. Mercury issues a signing secret once the webhook is registered — paste it here. Org-scoped.

### Configuration

- **`<owner-slack-id>` in `SOUL.md` and `channels/transactions.md`**: Replace with the Slack member ID of the person who should receive DMs for unknown vendors. Find it in Slack profile → "..." → "Copy member ID". Format: `U0123ABCD`.

### External Setup

- **Notion**: Create the vendor database (one row per recurring vendor — name is the only required column). Share the database with the integration created above. The agent treats any plausible search hit on the counterparty name as "known", so the database does not need a "Recurring" flag.
- **Mercury**: Configure a webhook subscription pointing at the URL printed by `valet channels create mercury-webhook`. Subscribe to transaction events at minimum. Paste Mercury's signing secret into `MERCURY_WEBHOOK_SECRET`.
- **Slack**: The org-level Slack connection must already exist (`valet channels create slack --org <org>`). Once the per-agent Slack bot is installed, invite it to `#finance` so it can post there. The bot must have permission to DM the account owner — the default Slack app scopes cover this.
