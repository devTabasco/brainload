---
name: brainload-baseline
description: >-
  Capture the intended architecture before AI implements it. Interviews the developer
  (or distills an existing PRD/plan document) into a compact baseline model —
  components, flows, invariants, acceptance criteria — saved to .brainload/baseline/
  as the baseline that /brainload later diffs against the actual implementation to
  show drift. Use before starting AI-assisted implementation of a feature, or when
  the user wants to write down the plan, PRD, or intended architecture first.
argument-hint: "[feature name, or path to an existing PRD/plan]"
---

# Brainload Baseline

A plan you wrote down is a plan you can drift-check. This skill externalizes the
developer's *intended* model before any code exists — that document becomes their
initial mental model, and later the baseline `/brainload` diffs reality against.

Your job here is to **capture, not design**. If the developer wants help designing,
that is ordinary conversation — have it first, then capture the result.

## Steps

### 1. Get the source

- A document path or pasted PRD given → distill it. Keep the developer's vocabulary.
- Otherwise, interview — **one compact message** asking for rough answers to:
  1. Goal in one sentence
  2. Non-goals (what this deliberately won't do)
  3. Expected components, and what talks to what
  4. State: what is stored, where, for how long
  5. Two or three invariants (must always / must never)
  6. How you'll know it works (acceptance checks)

  Rough answers are fine. Follow up **once**, only on empty answers. Do not
  interrogate at plan time — a rough correct sketch beats a beautiful wrong one.

### 2. Normalize into the baseline model

```markdown
---
name: <feature-slug>
date: <YYYY-MM-DD>
status: baseline
---

# <Feature> — baseline model

## Intent
<goal in ≤3 sentences, non-goals in one line>

## Expected shape
```mermaid
flowchart TD
    %% components and edges as the developer described them — rough is fine
```

## Expected flows
<1–2 flows, prose or sequence diagram>

## Planned invariants
1. …

## Acceptance
- …

## Open questions
- …
```

Keep the whole document under ~80 lines. It is a sketch of intent, not a spec.

### 3. Save

`.brainload/baseline/<feature-slug>.md`, following the same ask-once persistence
rule as `/brainload` (committed / gitignored / chat-only, recorded in
`.brainload/config.md`).

### 4. Close

One line: "When the implementation lands, run `/brainload` — it will diff reality
against this baseline."

## Rules

- Capture the developer's intent in their words; don't gold-plate it.
- Never block implementation on plan completeness — open questions belong in the
  Open questions section, not in more interview rounds.
- If a baseline for the same feature already exists, offer to update it rather
  than creating a duplicate.
