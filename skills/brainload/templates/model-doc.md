---
scope: <commits / range / PR / description>
date: <YYYY-MM-DD>
calibration: <familiarity from Phase 0>
status: pre-challenge   # pre-challenge | challenged
---

# <Feature or change name> — mental model

## The system in three sentences

<!-- Mechanism, not marketing. Test: reading only this, the developer should be able
     to guess where most bugs in this change would live. -->

## System map

<!-- One flowchart. Nodes = components (one level above functions), max 12.
     Edge labels = what flows (data/trigger), never "uses". Keep the classDefs and
     the legend line. -->

```mermaid
flowchart TD
    %% nodes and edges here
    classDef new fill:#d3f9d8,stroke:#2b8a3e
    classDef modified fill:#fff3bf,stroke:#e67700
```

Legend: green = new in this change · amber = modified · plain = pre-existing context

## Key flows

### <Flow 1 — primary happy path>

<!-- Sequence diagram. Number the steps; mark exactly where state changes
     (cache write, DB commit). -->

```mermaid
sequenceDiagram
```

### <Flow 2 — most instructive failure path>

```mermaid
sequenceDiagram
```

## Key concepts

<!-- 4–10 rows. "Why it exists here" = the forcing reason — the thing juniors are
     usually missing. This table is the challenge's coverage checklist. -->

| Concept | What it is | Why it exists here | Where |
|---|---|---|---|
| | | | `file:line` |

## Invariants

<!-- Only real ones: each has enforcing code, or is flagged
     "assumed, not enforced — fragile". Worth memorizing verbatim. -->

| # | Invariant | Enforced at | If broken |
|---|---|---|---|
| 1 | | `file:line` | |

## Design decisions

<!-- Infer honestly. If the reason isn't evident from code, write
     "reason not evident from code" — that's a WHY question, not a guess. -->

| Decision | Alternative not taken | Why this one | Revisit when |
|---|---|---|---|

## Failure modes & edge cases

<!-- Dependency-down matrix, boundaries, races. Note explicitly which are covered
     by tests and which are not. -->

| Scenario | Behavior | Tested? |
|---|---|---|

## Guided reading tour — the ~20% worth reading line-by-line

<!-- Ordered stops. Typical order: entry point → core decision → invariant
     enforcement → failure path. -->

1. `path/to/file:L10-L60` — <why this stop> — *notice:* <…> — *hold this question:* <…>

The remaining ~N lines are wiring and boilerplate: <one-line dismissal>.

## Prerequisites

<!-- Only techniques this developer flagged as unfamiliar. 3–5 lines each: what it
     is, the one property that matters here, where this change uses it.
     Delete the section if none. -->

## Self-check

<!-- 3–5 questions, no answers. Preview of the challenge. -->

1.

<!-- ## Challenge log — appended by Phase 4 after each challenge session:
     reconstructed mental model (concept tree with 🟢 solid / 🟡 shaky / ⚪ gap /
     🔴 misconception, coverage meter, ground-truth map annotated solid/gap/
     misconception), status table (solid / shaky / missing / confident-wrong /
     unverified), repairs made, date. -->
