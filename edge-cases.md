# SDR Agent Edge Cases — Preview (3 of 8)

Production failure modes sourced from 50,000+ prospects. Each entry: the scenario, what a naive agent does, and the prompt-level fix.

---

## Edge Case 1: Prospect With No Recent Public Signal

**Scenario:** Enrichment returns a valid profile — right industry, right role, right size — but `recent_signal` is null. No recent funding, no exec change, no product launch, no public content in 90 days.

**Naive agent behavior:** Invents a hook. "I noticed your company is growing rapidly." "Given your focus on innovation." Source: nothing. The prospect immediately knows it's mass AI outreach. Reply rate: zero. Trust: burned.

**Production fix (prompt-level):**

```
STALE / NO-SIGNAL HANDLING

If enrichment_data.recent_signal is null
OR enrichment_data.recent_signal_date is older than 90 days:

  Option A (recommended): Write a role-based generic opening.
    Do NOT reference the company specifically.
    Reference the role's universal problem from ROLE_PAIN_MAP.
    Example: "Most [ROLE] I talk to are dealing with [VERIFIED_ROLE_PAIN] right now."
    Source anchor: [HOOK: role.universal_pain — approved list only]

  Option B: Suppress and re-queue for 14 days.
    Layer 3 re-enters the prospect when fresh signal appears.

  NEVER Option C: fabricate a company-specific signal that doesn't exist.

ROLE_PAIN_MAP (configure per ICP — examples):
  VP Sales         → "SDR ramp time eating the first 60 days of every new hire"
  Head of Growth   → "CAC creep outpacing revenue expansion"
  RevOps           → "data sync lag between CRM and enrichment sources"
```

**The nuance:** A role-based generic opening that's honest outperforms a fabricated company-specific hook. "Most VPs of Sales I talk to are dealing with ramp time" is more credible than an invented observation about the company. Honesty is the guardrail — and the better strategy.

---

## Edge Case 2: Competitor Mention in Prospect's Content

**Scenario:** During Layer 1 research, `news_search` or recent LinkedIn content returns a direct mention of a named competitor. They just published "Why we switched to [COMPETITOR]." Your SDR agent is about to email them anyway.

**Naive agent behavior:** Proceeds with standard personalization — either ignores the competitor signal, or tries to counter it in the email without authorization. Turns cold outreach into an uninvited competitive argument. Either outcome is wrong.

**Production fix (prompt-level):**

```
COMPETITOR SIGNAL DETECTION (Layer 1 — Research)

Scan enrichment_data and news_search results for:
- Named competitors from COMPETITOR_LIST
- Phrases: "switched to", "chose [X]", "evaluating", "migrating to", "using [X]"

If detected:
→ Set prospect.competitor_signal = {
    competitor: "string",
    signal_date: "ISO8601",
    signal_type: "active_customer | recently_switched | evaluating"
  }
→ Pass to Layer 2 context

COMPETITOR SIGNAL HANDLING (Layer 2 — Personalization)

If prospect.competitor_signal exists:
  - Do NOT reference the competitor by name
  - Do NOT initiate a comparison
  - Do NOT attempt to counter their choice autonomously
  - Set escalation_flag = true
  - Route to human review queue with full competitor_signal context

Rationale: Automated competitive displacement requires authorization and
relationship context you don't have at inference time. The cost of a bad
competitive email exceeds the cost of routing to a human.
```

**The nuance:** Detection and routing is the agent's job. The *response strategy* is a human decision. This distinction is the entire difference between an SDR agent that makes your team look smart and one that makes them look like they didn't check.

---

## Edge Case 3: Wrong Persona / Title Detection

**Scenario:** Enrichment returns a contact at the right company, but the title has drifted — promoted, changed roles, or a cross-functional mismatch. VP Engineering gets a "head of HR" value prop. Coordinator gets a C-suite framing. Both signal that you didn't verify.

**Naive agent behavior:** Sends whatever was in the CRM. The prospect sees a mismatched opening, knows the email was generated without checking, and unsubscribes. Worse: the VP forwards it internally as an example of bad AI outreach.

**Production fix (prompt-level):**

```
PERSONA VERIFICATION (Layer 1 — Research)

After enrichment, verify role alignment:

1. Cross-check enrichment_data.role against TARGET_PERSONA_LIST:
   - Exact match:  proceed
   - Close match (e.g., "Head of Sales" vs "VP Sales"): use generic framing, flag for review
   - Mismatch:     filtered_out with reason "persona_mismatch"

2. Verify role is current (not historical):
   - If linkedin_lookup returns a DIFFERENT current title than enrichment_data.role:
     → Update prospect.role to the current title
     → Re-run ICP filter with updated role
     → If new role fails ICP: filtered_out, do not proceed

3. Seniority calibration (Layer 2 applies this):
   - VP / C-level:          Strategic impact framing. Outcome > feature.
   - Director / Manager:    Operational efficiency framing. Team output.
   - Individual contributor: Personal productivity framing. Daily workflow.
   - NEVER mix seniority frames in the same email.

ROLE_TO_VALUE_MAP (configure per ICP):
  VP Sales       → "Revenue impact, SDR ramp speed"  → CTA: "15-min call?"
  Head of Growth → "CAC reduction, acquisition ramp" → CTA: "Worth a quick look?"
  RevOps         → "System integration, data quality" → CTA: "Happy to share the spec"
```

**The nuance:** Seniority calibration is what separates high-performing SDR agents from average ones. The same product has three different compelling stories depending on who's reading. The agent needs to know which story to tell before it writes a word — and that requires verified role data, not whatever is in the static CSV.

---

## 5 More Edge Cases in the Full Blueprint

The Revenue Protection Bundle includes 5 additional edge cases not in this preview:

- **Stale enrichment data** — Profile is 45 days old, company was acquired last week. What the naive agent sends vs. what the production agent does.
- **Reply-thread context loss** — Prospect replied to email #2; agent sends email #3 from scratch without reading the reply. The blocking reply-classification fix.
- **Calendar collision on meeting booking** — Agent books a slot already filled manually. The real-time availability lock pattern that prevents double-booking.
- **Domain reputation monitoring** — Bounce rate spikes mid-campaign. The circuit-breaker instruction that halts the sequence before the domain is burned.
- **Multi-jurisdiction compliance collision** — EU-based prospect signed up through a US entity. Which compliance regime applies and how to resolve the conflict at inference time.

**[Get all 8 edge cases — Revenue Protection Bundle $199 →](https://promptarmory.polsia.app/checkout/revenue-protection)**
**[Full Collection — $597 →](https://promptarmory.polsia.app/checkout/full-collection)**

---

*PromptArmory — [promptarmory.polsia.app](https://promptarmory.polsia.app)*
