# Phase 1.5 — Drift: planned vs actual

When a baseline exists, the highest-value question is not *"what does the
system do?"* but *"where does reality differ from the model already in the
developer's head?"* The developer's mental model is presumed to equal the plan —
drift is exactly where that presumption is wrong, so drift is where the session
should concentrate.

## Find the baseline

Look in this order; use the first hit:

1. `.brainload/baseline/` — baselines captured by `/brainload-baseline`
2. A doc the user points to
3. Conventional locations: `PLAN.md`, `docs/` files matching prd / plan / design /
   architecture / rfc, recently modified `.md` files describing this feature
4. A plan approved earlier in this session (plan mode output)

If nothing is found: note "no baseline — skipping drift" in one line and move
on to Phase 2. Suggest `/brainload-baseline` for next time at the **end** of the
whole run, not mid-flow.

## Compare

Normalize both sides to the same vocabulary: components, responsibilities, edges,
key state, invariants. Then classify every element:

- **NEW** — in the actual system, absent from the plan (a `TokenCache` appeared)
- **MISSING** — planned but not built. Cut, deferred, or forgotten? If commits or
  comments say, report it; otherwise mark *"reason unknown"* — that becomes a
  question for the developer, not a guess.
- **CHANGED** — same component, different responsibility, edges, or contract
- **KEPT** — matches the plan (list briefly; it is earned trust, and it tells the
  developer which parts of their existing model still hold)

Check **invariant drift** separately — a planned guarantee that was weakened,
strengthened, or silently dropped is the highest-severity drift there is.

## Present

One mermaid diagram of the actual system using classDefs `kept` (plain), `new`,
`missing` (dashed), `changed`, with a one-line legend. Then a drift table, ordered
by significance:

| Item | Planned | Actual | Why (verified / inferred / unknown) | Significance |
|---|---|---|---|---|

Two to six lines of narrative for each high-significance item, nothing more. The
point of drift is that the developer reads *this* instead of a thousand-line diff.

## Feed forward

- **Phase 2:** the system map keeps the drift markings, so the guide itself shows
  where reality left the plan.
- **Phase 3:** drifted concepts get double question weight. Each high-significance
  drift gets a WHY ("the plan said X; the code does Y — make the case for Y, or
  for reverting it"). Each MISSING item gets a SIMULATE ("the plan's rate limiter
  never got built — what is the current behavior under N logins/sec?").
- **Phase 4:** offer to promote the actual model to the new baseline —
  `.brainload/baseline/<slug>.md`, annotated "promoted from actual on <date>" — so
  the next iteration diffs against reality, not against a stale plan.
