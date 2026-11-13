# HANDOFF — read this fully before any work

Generated: <ISO timestamp> · Git HEAD: <short SHA, or "no-git">
To resume: run /handoff — it re-verifies this file against the current repo before proceeding.
Trust rule: `[V]` = verified against the repo during the handoff run. `[?]` = unverified memory — treat as a lead, re-check before relying on it.

<!-- If many claims below are [?], add here: "Low-confidence handoff — verify aggressively." -->

## 1. Current goal

<!-- ≤5 lines. The single objective in the user's terms + definition of done.
GOOD: "Make POST /orders idempotent; done when a duplicate Idempotency-Key returns the original 200 and writes no second row. [V] test file exists: orders.idempotency.test.ts"
USELESS: "Working on the orders feature." -->

## 2. Verified state — where the work ACTUALLY stands

<!-- Ground truth, not intended truth. Each line = claim + evidence + tag.
GOOD: "[V] 41/43 tests pass; failing: order.retry (assertion line 88), order.concurrency (timeout). Output tail pasted below."
GOOD: "[V] Idempotency middleware half-implemented in src/mw/idempotency.ts:1-60 — key extraction done, storage lookup is a TODO stub at line 47."
GOOD: "[?] I believe the Redis client is wired in app.ts but did not re-open it — verify."
USELESS: "Made good progress, most things work now." -->

### Test/build output (tail, from this handoff run)

```
<paste the actual tail here — never from memory>
```

## 3. Decisions made & WHY

<!-- Decision + reason + rejected alternative. The WHY is what stops the next session relitigating.
GOOD: "Idempotency keys in Redis with 24h TTL, NOT Postgres. Why: hot path can't afford a sync DB write; Postgres unique-constraint can't express TTL cleanly. [V] reflected in idempotency.ts imports."
USELESS: "Decided to use Redis." -->

## 4. Failed approaches / dead ends — DO NOT RETRY THESE

<!-- The most valuable section. Each entry: what was tried → exactly how it failed → the specific reason → don't-retry note.
GOOD: "Tried SETNX for the idempotency lock → race: two concurrent requests both saw no key and both wrote. Reason: SETNX + separate EXPIRE isn't atomic; crash between them orphans the lock. DO NOT reintroduce two-step lock-then-expire — use single-call SET key val NX EX 86400. [V] race reproduced by order.concurrency test."
USELESS: "Tried some locking, didn't work." -->

## 5. Known traps in this codebase

<!-- Non-obvious facts that would bite a stranger.
GOOD: "[V] src/legacy/mailer.ts looks dead (no static imports) but is loaded dynamically via require(`./legacy/${name}`) in plugins.ts:112 — deleting it breaks email at runtime, not build."
USELESS: "Be careful with the config files." -->

## 6. Next steps (ordered)

<!-- Numbered; each concrete enough to start without re-exploring: file, entry point, chosen approach.
GOOD: "1. Implement the storage lookup stub at idempotency.ts:47 using SET ... NX EX (see dead-end #1). 2. Re-run npm test -- order.concurrency; must stop timing out. 3. Only then wire middleware into routes/orders.ts (currently unregistered)."
USELESS: "Finish the feature and test it." -->

## 7. Open questions for the user

<!-- Decisions the model cannot make alone; the next session must NOT guess past these.
GOOD: "Scope idempotency keys per-user or global? Affects Redis key format. Assumed per-user for now [?] — confirm before shipping."
USELESS: "Let me know if you have questions." -->

<!-- FINAL CHECKS before saving:
1. Delete all HTML comments (including this one).
2. Every claim tagged [V] or [?]. [V] only if verified during THIS handoff run.
3. ≤250 lines target, 400 hard ceiling. Cut prose and compress Decisions first; never cut sections 2, 4, 5, 6.
4. Apply to each entry: "Would a stranger with only this line avoid the mistake we made?" -->
