# Phase 2 — Mental model & learning guide

Goal: a model small enough to hold in one head and sharp enough to predict with.
The document does double duty: it is the learning guide the developer reads now, and
the coverage checklist the challenge phase interrogates next.

Build it from `templates/model-doc.md` (relative to this skill's directory). Fill
every section; if a section genuinely does not apply, delete it and leave a one-line
reason in its place. Never ship placeholders.

## Section guidance

1. **The system in three sentences.** Mechanism, not marketing. Name what talks to
   what and where the crux is. Quality test: after reading only this paragraph, the
   developer should be able to guess where most bugs in this change would live.

2. **System map.** One mermaid flowchart. Nodes are components — one abstraction
   level above functions (services, modules, stores), unless the change is small
   enough that functions *are* the components. Twelve nodes maximum; if you need
   more, split into an overview plus one zoom. Label edges with what flows (data or
   trigger), never with "uses". Mark the change surface with the template's
   classDefs: `new`, `modified`, plain = pre-existing context — the developer should
   see at a glance where the change actually landed. Include the legend line.

3. **Key flows.** One to three mermaid sequence diagrams: the primary happy path,
   the most instructive failure path, and — if the change has one — the trickiest
   lifecycle (rotation, invalidation, retry, reconciliation). Number the steps and
   mark exactly where state changes (a cache write, a DB commit) — those are the
   points the developer must be able to reproduce from memory.

4. **Key concepts.** A table of 4–10 rows: concept, what it is in one line, *why it
   exists here* (the forcing reason — this is what juniors are usually missing), and
   where it lives (`file:line`). This table is the challenge's coverage checklist:
   every row will get at least one question.

5. **Invariants.** "Always/never" statements with the enforcement site (`file:line`)
   and the concrete symptom if broken. Only real ones — each must either have
   enforcing code or be explicitly flagged *"assumed, not enforced — fragile"*.
   Invariants are the part of the doc worth memorizing verbatim.

6. **Design decisions.** Decision, the alternative not taken, why this one won, and
   when to revisit. Infer honestly from code and commits; if the reason is not
   evident, write *"reason not evident from code"* — that becomes a WHY question
   for the developer (who may know context you don't), never something to invent.

7. **Failure modes & edge cases.** A short dependency-down matrix (each external
   dependency: down / slow / garbage → observed behavior), boundary conditions, and
   an explicit note of which of these are covered by tests and which are not.

8. **Guided reading tour.** The ordered stops from the Phase 1 reading list:
   `file:lines` — why this stop matters — what to notice — a question to hold in
   mind while reading. Then close the section with one honest line dismissing the
   rest: "The remaining ~N lines are wiring and boilerplate: <one-line summary>."
   This section is the answer to "do I have to read all 1,300 lines?" — no, these
   ~260, in this order.

9. **Prerequisites.** Only for techniques the Phase 0 calibration flagged as
   unfamiliar (JWT structure, Redis TTLs, optimistic locking, …). Three to five
   lines each: what it is, the one property that matters here, and where this change
   uses it. Delete the section for developers who need nothing.

10. **Self-check.** Three to five questions, no answers, previewing the challenge.
    They prime retrieval — the developer who tries them before saying "challenge me"
    learns more from Phase 3.

## Calibration effects

- **Junior / new to the area:** define every term on first use, keep the
  prerequisites section, more tour stops with narrower line ranges, flows explained
  beneath each diagram.
- **Senior / wrote-this-by-hand-before:** drop prerequisites, compress concepts,
  spend the space on decisions, failure modes, and anything non-obvious about *this*
  implementation.

## Mode effects

- `--quick`: elevator model, system map, concepts, invariants, reading tour only.
  Target ≤150 lines.
- *default*: all sections, target ≤300 lines.
- `--deep`: everything, plus a zoom diagram per major flow. Target ≤500 lines.

## Diagram rules

- Plain, widely-supported mermaid only — no experimental syntax.
- Node ids and labels use the real component names from the code.
- Edge labels name the payload or trigger (`credentials`, `refresh token`,
  `SETEX session:*`), not generic verbs.

## Presenting

Present the guide in chat — one message if it fits comfortably, otherwise map and
flows first, the rest second. No filler commentary around it. End with exactly two
offers: walk any part of the reading tour together, or start the challenge
("say **challenge me**"). Do not start Phase 3 without an explicit go.

### Rendering — terminals don't draw mermaid

Assume the chat surface shows diagram *source*, not diagrams. Compensate, in order:

1. **ASCII first in chat.** For small diagrams (≤6 nodes), lead with a compact
   ASCII sketch; keep the mermaid block beneath it for the saved doc. For larger
   maps, give a one-line ASCII spine (`GUI → API → parser → catalog → sources`)
   above the mermaid block so the shape is graspable without rendering.
2. **Publish if a rendering surface exists.** If an artifact/page-publishing tool
   is available in the session, publish the saved model doc there after Phase 2
   and hand the user the link — mermaid renders natively on such pages.
3. **Otherwise, save an HTML sibling.** Next to the saved `.md`, write
   `<slug>.html`: a minimal page containing the doc with each diagram in a
   `<pre class="mermaid">` block and a mermaid CDN `<script>` at the end. Offer
   (don't auto-run) `open`/`xdg-open` on it, and mention that the `.md` also
   renders on GitHub and in IDE markdown previews.

Never drop the diagrams because the surface is poor — the saved doc is the durable
artifact, and the challenge references the map by node names.

## Saving

Follow the saving protocol in SKILL.md (ask once per repo: committed / gitignored /
chat-only; record in `.brainload/config.md`). Save as
`.brainload/models/YYYY-MM-DD-<slug>.md` with frontmatter filled in and
`status: pre-challenge`. Phase 4 will update it.
