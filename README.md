# Transaction Watchdog

A quiet eye on every [Mercury](https://mercury.com) payment over $1,000. For every outgoing payment of $1,000 or more, the agent searches your Notion vendor database for the counterparty. Known vendor — a one-line note in `#finance`. Unknown vendor — a direct message to the owner with the amount and counterparty.

## Prerequisites
- A [Mercury](https://mercury.com) account with webhook access (Settings → Developers → Webhooks)
- A [Notion](https://notion.so) vendor database shared with an internal integration
- A Slack workspace where you can invite the agent's bot

<table>
  <tr>
    <td><strong>CHANNELS</strong></td>
    <td><code>mercury-webhook</code> · <code>slack</code></td>
  </tr>
  <tr>
    <td><strong>CONNECTORS</strong></td>
    <td><code>notion-mcp</code></td>
  </tr>
  <tr>
    <td colspan="2" align="center">
      <br />
      <a href="https://dashboard.valet.dev/setup/configure?from=github.com/valet-agents/mercury-transaction-watchdog">
        <img src="https://raw.githubusercontent.com/valet-agents/mercury-transaction-watchdog/main/.github/deploy-button.svg" alt="Deploy Agent →" height="40" />
      </a>
      <br /><br />
    </td>
  </tr>
</table>
