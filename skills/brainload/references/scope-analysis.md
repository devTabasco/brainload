# Phase 1 — Scope analysis

Goal: know what was actually built — well enough to teach it, and well enough to
write challenge questions whose answers you have verified in the code.

## 1. Resolve the scope

| Input | How to resolve |
|---|---|
| *(no scope, code was written in this session)* | The files you changed this session. Verify against reality: `git status --porcelain`, `git diff --stat` for uncommitted work, plus any commits made during the session. |
| *(no scope, fresh session)* | Dirty tree → working tree vs `HEAD`. Clean tree → ask: last commit, a range, a PR, or a described feature? |
| `<sha>` | `git show --stat <sha>`, then `git show <sha>` |
| `<a>..<b>` | `git log --oneline <a>..<b>`, `git diff <a>..<b>` |
| `#N`, a PR URL, or "the PR" | `gh pr view <N> --json title,body,baseRefName,headRefName,files` and `gh pr diff <N>`. If `gh` is unavailable, ask for an equivalent ref range. |
| Natural language ("the auth feature") | Find candidates: `git log --oneline -30`, `git log -i --grep=<keyword>`, `git log -S<symbol> --oneline`. Propose the inferred commit range and **get confirmation before deep reading** — a wrong scope wastes the whole session. |

Always confirm the resolved scope in one line before analyzing:

> Scope: 4 commits, 17 files, +1,328 −204 — JWT auth with Redis sessions. Proceeding.

For explicit refs, confirm-and-proceed. For natural-language scopes, wait for a yes.

## 2. Read the change

Work in this order:

1. **Shape first.** `git diff --stat <scope>`. Cluster files by area. Separate
   source / tests / config / generated / lockfiles.
2. **Classify each file:**
   - *load-bearing* — new behavior, decisions, algorithms
   - *interface* — contracts: API shapes, schemas, events, types
   - *wiring* — DI registrations, routes, exports, imports
   - *boilerplate/generated* — scaffolding, migrations dumps, lockfiles
   - *tests* — read the assertions: tests are the spec the implementation was
     accepted against, and they encode intended edge-case behavior
3. **Read load-bearing and interface files properly.** For new or heavily changed
   files, read the current file, not just the hunks — hunks hide the shape. For
   lightly changed files, hunks plus surrounding context are enough.
4. **Follow one level outward.** For each new edge into pre-existing code, open the
   callee far enough to know its contract. The change's meaning often lives at its
   boundary with old code.
5. **Large scopes** (roughly >2,000 changed lines): read every interface and entry
   point fully; read hunks for the rest; keep an explicit list of what you skimmed
   or skipped. You will disclose it. If a read-only subagent is available, you may
   delegate bulk file reading — but conclusions still need `file:line` evidence.

## 3. Extract the analysis notes

Build internal notes (not shown to the user in raw form). Every item carries
`file:line` evidence:

- **Component inventory** — name, one-line responsibility, and whether it is
  `new` / `modified` / pre-existing `context`
- **Edges** — who calls whom, with what data, sync or async
- **Entry points & triggers** — routes, handlers, jobs, consumers, CLI commands
- **State** — what is stored where (tables, caches, memory), lifetimes and TTLs,
  who owns invalidation
- **Contracts** — API shapes, events, schemas; compatibility implications
- **Invariants** — what must always/never hold, *where each is enforced*, and the
  concrete symptom if violated
- **Decisions** — places the implementation chose between real alternatives; note
  the alternative not taken
- **Failure paths** — behavior when each external dependency is down, slow, or
  returns garbage
- **Config & env** — new knobs, defaults, prod-vs-dev differences
- **Security-sensitive spots** — authn/z, secrets, PII, injection surfaces
- **Concurrency & ordering** — races, idempotency, retries, exactly-once claims
- **Test coverage map** — which of the above are tested and which are not.
  *Untested invariants are prime challenge material.*
- **Reading list** — the ordered ~20% worth line-level reading. Each stop: file and
  line range, why it earns a stop, what to notice there. Typical order: entry point
  → core decision → invariant enforcement → failure path.

## 4. Honesty requirements

- Never record an invariant, edge, or behavior you have not seen enforced in code.
- Distinguish *"the code does X"* (verified, cite lines) from *"the code appears to
  intend X"* (unverified). Only verified facts may feed challenge answer keys.
- If session memory conflicts with the files on disk, the files win — reread.
- Boilerplate ratio matters: note roughly how much of the diff is mechanical, so the
  guide can honestly say "the other N lines are wiring."
