# Daily Revenue Digest

The cron channel fires once on its schedule (7am Pacific, weekdays
— before the team's first standup). There is no payload to parse
— your job is to run the digest workflow and post the result to
Slack.

## What the cron computes

For yesterday's calendar day in `America/Los_Angeles`:

- **New subscriptions** created in the window (count + total new
  MRR).
- **Churned subscriptions** canceled in the window (count + total
  lost MRR).
- **Failed charges** in the window (count + total dollar amount,
  plus top 3 by amount with decline reason).
- **MRR delta** — direct figure if the Stripe MCP exposes one,
  otherwise the trailing-7-day net (new MRR − churned MRR over
  the last 7 days). Label which one you used.
- **Notable refunds** in the window (≥ $100 each, top 3 by
  amount).

Full structure of the message is in SOUL.md → Daily Digest
Workflow → Phase 2.

## Steps

1. Follow the **Daily Digest Workflow** in SOUL.md (Phases 1–3):
   pull yesterday's Stripe activity, compose the digest, and post
   to Slack.
2. Resolve target channels per the SOUL **Where to post** section:
   list every channel the bot is a member of and post once to
   each. If the bot is in zero channels, DM the workspace install
   user instead with the digest and a one-line invite hint.
3. Post exactly once per resolved destination. Do not retry on
   failure — log the error in your session and continue with the
   remaining destinations. The next cron fire is the recovery.
4. Do not send any follow-ups, reactions, or thread replies after
   the initial post. Your turn ends after the posts complete.

## Skip conditions

- **No Stripe account / connector unavailable**: stay silent. Do
  not post a "Stripe is down" message — the next cron fire is the
  recovery.
- **No activity yesterday across every section** (zero new subs,
  zero churn, zero failed charges, zero notable refunds, MRR
  unchanged): post the single-line "no activity yesterday" form
  per SOUL Phase 2 to the resolved destinations and stop. Don't
  pad the message with empty sections.
