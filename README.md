# Stripe Revenue Health

Daily revenue digest — new subs, churn, failed charges — posted to your finance channel before the team's first standup.

## Prerequisites
- A [Stripe](https://stripe.com) account with permission to create a [Restricted Key](https://dashboard.stripe.com/apikeys) (read-only scopes are enough)
- A Slack workspace where you can install the agent's bot and invite it to one or more channels

<table>
  <tr>
    <td><strong>CHANNELS</strong></td>
    <td><code>slack</code> · <code>cron</code> — 7am weekdays</td>
  </tr>
  <tr>
    <td><strong>CONNECTORS</strong></td>
    <td><code>stripe-mcp</code></td>
  </tr>
  <tr>
    <td colspan="2" align="center">
      <br />
      <a href="https://valet.dev/deploy?from=github.com/valet-agents/stripe-revenue-health">
        <img src="https://raw.githubusercontent.com/valet-agents/stripe-revenue-health/main/.github/deploy-button.svg" alt="Deploy Agent →" height="40" />
      </a>
      <br /><br />
    </td>
  </tr>
</table>
