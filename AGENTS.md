This folder contains the source for a Skilled Agent originally built for the Valet runtime. Changes should follow the Skilled Agent open standard.

## Setup

### Connectors

- **stripe-mcp**: The Stripe MCP server. The agent uses it to read customers, subscriptions, charges, refunds, invoices, and disputes for the daily revenue digest and ad-hoc Slack questions, and (only when explicitly asked and confirmed in Slack) to issue refunds, credits, or subscription cancels. Add it from the catalog at the org level so other Stripe-powered agents can share it.

### Channels

- **slack** (slack): The agent's per-agent Slack bot. Listens for @mentions and replies in-thread, and posts the daily revenue digest to whichever channels the bot has been invited to. Slack writes use the auto-injected outbound Slack connector.
- **cron** (cron): Fires the daily revenue digest at 7am Pacific, Monday through Friday (`0 7 * * 1-5`, `America/Los_Angeles`) — chosen to land before most teams' first standup. Declared inline in `valet.yaml`, so it's created automatically by the dashboard setup flow.

### Secrets

- **STRIPE_SECRET_KEY** (required): Stripe API key in the form `sk_live_...` or `rk_live_...`. **Strongly recommended:** generate a [Restricted Key](https://dashboard.stripe.com/apikeys) with read-only scopes on Customers, Subscriptions, Charges, Refunds, Invoices, and Disputes — that is enough for the daily digest and the read-only Slack Q&A. A full secret key (`sk_live_...`) also works, but the restricted form is safer because it limits the blast radius if the key ever leaks. Store at the org level so future Stripe-powered agents can share it.

If you intend to actually let teammates run write actions (refunds, subscription cancels, credits) from Slack, the Restricted Key needs the matching write scope on those resources. The SOUL still requires confirm-then-execute in-thread for every write.

### External Setup

1. After deploy, invite the agent's Slack bot to whichever channel(s) you want the daily revenue digest in — typically a finance, founders, or revenue channel. The agent posts the digest to every channel it's a member of. If the bot has not been invited anywhere, the digest is sent as a DM to the workspace install user with a one-line nudge to invite it somewhere.
2. Invite the bot to any additional channels where teammates should be able to @mention it for ad-hoc revenue questions (e.g. *"how's MRR tracking this week?"*).
3. The first cron fire is the next 7am Pacific weekday after deploy. To smoke-test sooner, @mention the bot in Slack with a question like *"how many failed charges yesterday?"* — that exercises the Slack + Stripe path without waiting for the cron.

## Customizing

- **Change the schedule**: edit the `cron` and `timezone` on the `cron` channel in `valet.yaml`, then redeploy. 7am Pacific weekdays is the default because it lands before most teams' first standup; move it earlier or later to match your team's rhythm.
- **Change what's in the digest**: edit the **Phase 1** and **Phase 2** sections of `SOUL.md`. The default sections are new subs, churn, failed charges, MRR delta, and notable refunds — add disputes, trial conversions, or top customers if your team cares about them.
- **Control where the digest posts**: invite or remove the bot from channels in Slack — that's the only signal the agent uses. There is no channel name in the configuration.
- **Tighten the key**: if the team will only ever read from Stripe via this agent, narrow the Restricted Key to the six read scopes listed above and remove every write scope. The agent will fail loudly on any write attempt — that's by design.
