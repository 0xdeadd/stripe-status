# stripe-status

> Check Stripe payments, subscriptions, refunds, balances, and listen to webhooks from the terminal.

An [OpenClaw](https://github.com/0xdeadd/openclaw) skill that wraps the [Stripe CLI](https://docs.stripe.com/stripe-cli) into a set of recipes for the most common "what's going on in my Stripe account right now" questions, runnable directly from your terminal or by your AI agent.

## What it does

Pulls the data that operators actually need without clicking through the Stripe dashboard:

- **Balance**: `available` + `pending` at a glance
- **Payments**: recent successes, recent failures, lookup by `pi_*`
- **Subscriptions**: active, past-due, canceled, lookup by `sub_*`
- **Refunds**: recent and by id
- **Customers**: recent, by email, by `cus_*`
- **Webhooks**: live-listen against a local dev server, trigger test events, tail API logs
- **Events**: recent and filtered by type

## Install

Via OpenClaw (recommended):

```bash
openclaw install 0xdeadd/stripe-status
```

Or manually — the skill just shells out to the Stripe CLI, so make sure it's installed:

```bash
brew install stripe/stripe-cli/stripe
stripe login
```

Then drop `SKILL.md` into your `~/.openclaw/skills/` (or wherever your skill registry lives) and you're ready.

## Examples

```bash
# Pre-payment debug: did this customer actually pay?
stripe payment_intents retrieve pi_3RxYz...

# Reconciliation: any failed charges in the last hour?
stripe payment_intents list --limit 25 --status requires_payment_method

# Membership churn check
stripe subscriptions list --status past_due --limit 10

# Live-listen for webhooks while you develop your handler
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

The full command reference is in [`SKILL.md`](./SKILL.md).

## When to use this vs the Stripe Dashboard

This skill is for **read-only operator inspection from the terminal**, often from inside an AI agent loop. Use the Stripe Dashboard for:

- PCI / compliance work
- Modifying the product / price catalog
- Long-form accounting and tax exports
- Anything that involves a chargeback dispute UI

Use the Stripe SDK (not this skill) for:

- Building real integrations (checkout, subscriptions, Connect, Terminal)
- Anything that mutates state in production

## Built for

[OpenClaw](https://github.com/0xdeadd/openclaw) is a personal-AI-assistant framework. This is one of its skills — a small, single-purpose capability the agent can pick up to answer a class of questions. The skill manifest (front-matter in `SKILL.md`) tells OpenClaw what binaries are required, how to install them, and when the skill should and shouldn't fire.

## Author

Built by **Clinton Phillips** ([clintphillips.dev](https://clintphillips.dev)) while running Stripe in production on [book.zanysplayworld.com](https://book.zanysplayworld.com). Real receipts: $76,725 processed, 1,647 customers, 0.05% refund rate over 180 days.

## License

MIT.
