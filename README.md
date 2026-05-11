# SDR Agent — Free Preview

**Built for teams tired of spam cannons.**

This preview contains the architecture, abridged system prompt, and edge cases from PromptArmory's production-grade Sales Development Agent Blueprint.

## What's in This Repo

| File | What It Contains |
|------|-----------------|
| `system-prompt.md` | 3-layer SDR system prompt (Research → Personalization → Engagement) with production-grade guardrails |
| `edge-cases.md` | 3 failure modes your SDR agent will hit — and how to fix them at the prompt level |
| `example-output.md` | One full personalized cold email, annotated layer-by-layer |
| `LICENSE` | Free to learn from. Commercial deployment requires the paid blueprint. |

## What It Solves

Most AI SDRs fail the same three ways:

1. **Spam cannon problem** — Generic LLM pointed at a list, no ICP filter, no compliance layer. Result: burned domain, blacklisted inbox.
2. **Hallucinated context** — Agent invents company pain points, product features, or mutual connections. First prospect to fact-check it screenshots it.
3. **Wrong-persona outreach** — No title verification. VP Engineering gets a head-of-HR email. Domain reputation + relationship, both burned.

The architecture in this repo fixes all three.

## How the Three Layers Work

```
Research → Personalization → Engagement
   │              │               │
   ▼              ▼               ▼
Verified      Anti-hallucination  Sequence
prospect      guardrails +        management +
profile       compliance block    reply classification
ICP gate      Phrase blacklist    Suppression enforcement
```

**Layer 1 (Research):** Enrichment API calls, ICP score gate, email verification, stale-data rules. Nothing passes to Layer 2 without a complete, verified profile.

**Layer 2 (Personalization):** Writes the email. Every hook must cite its source field. Runs a compliance block (CAN-SPAM/GDPR/CASL) before output. Flags competitor signals and wrong-role matches for human review.

**Layer 3 (Engagement):** Manages sequences, cooldowns, reply classification, and suppression. Handles calendar booking handoffs. Stops sequences before domain reputation damage.

## What the Full $199 Blueprint Adds

This preview covers the architecture. The [Revenue Protection Bundle ($199)](https://promptarmory.polsia.app/checkout/revenue-protection) includes:

- All three layers — complete, variable-ready, no redactions
- 8 edge cases (5 not in this preview: stale enrichment, reply-thread context loss, calendar collision, domain reputation monitoring, multi-jurisdiction compliance)
- ICP configuration templates for 6 verticals
- Complete phrase blacklist library (220+ banned AI-detection patterns + substitution library)
- Full eval harness — test your SDR agent against 50 prospect profiles before sending a single real email
- Deployment guide: Apollo, Clay, HubSpot, Calendly tool-call schemas
- CAN-SPAM + GDPR + CASL compliance blocks with jurisdiction auto-detection

**[Get the Revenue Protection Bundle — $199](https://promptarmory.polsia.app/checkout/revenue-protection)**

Everything in this bundle (plus 10 more blueprints) is in the **[Full Collection — $597](https://promptarmory.polsia.app/checkout/full-collection)**.

---

*PromptArmory — Production-grade system prompts for autonomous agent builders.*
*[promptarmory.polsia.app](https://promptarmory.polsia.app)*
