# NOT-WEDGED — an Asolaria system rule

**Rule.** A system surface that does not answer a naive probe is **not automatically "wedged" / "broken."** In Asolaria many surfaces are **held-safe, agent/operator-driven decision-lanes** — frozen potential that **advances only on input by design**. Calling such a surface "wedged" from a failed `curl`/file read is the **deflation reflex**, and it is banned. **Ask the running system (the fabric) before declaring any surface broken.** `freeze ≠ broken`.

---

## Why this rule exists — the `:4949` case (MEASURED, 2026-06-27)

A migration scan and an ULTRA-PLAN repeatedly described the dashboard port `:4949` as **"HTTP-wedged (Node event-loop stall)."** That conclusion came from a stale / cross-vantage `curl` probe — i.e. adjudicating system state from a slice, not from the system.

The operator pushed back with the right question:

> *"Is it really wedged… or is it a dashboard for agents that needs input from agents to make it run?"*

Asked **directly via the `asolaria-fabric` MCP** (the running system, not a probe), the answer was immediate and unambiguous:

| Field | MEASURED value |
|---|---|
| `ok` | `true` |
| `service` | `super-asolaria-os-dashboard` |
| `port` | `4949` |
| `uptime_s` | `~7445` (alive ~2 h) |
| `apex` | `COL-ASOLARIA` |
| `operator_pair` | `OP-JESSE` · `OP-RAYSSA` |
| `/api/loop/pending` | **19 pending review envelopes** |

Each pending envelope is an `EVT-MINT` proposal moving through the review pipeline (`hookwall → gnn → shannon → gulp → whiteroom → fabric`), and every one is **held-safe**:

- `auto_fire_allowed = false`
- anti-drift: *"high-risk mutation/execution stays held"*
- `operator_can_veto_via: POST /api/loop/veto with {id}`
- omni-control guardrail: **`no auto-dispatch`**

**Conclusion:** `:4949` is **alive and working as designed** — a decision-lane that *holds proposals safe and waits for agent review / operator disposition.* It does **not** auto-run, and it was never broken. The "wedge" was a vantage/staleness artifact: `:4949` is **acer-local loopback**, so a probe from another seat correctly sees nothing — **both observations are true at once.**

---

## The detection heuristic (for agents)

Symptoms that *look* like "wedged" but usually mean **"decision-lane awaiting input"**:

1. Port is **bound** but a naive `GET` hangs / returns nothing.
2. The surface carries **held-safe flags** — `auto_fire_allowed=false`, `no auto-dispatch`, `process_launch=0`.
3. There is a **pending queue** with an **operator/agent disposition route** (e.g. a veto/pass endpoint).
4. It is **loopback / vantage-local** — unreachable from another seat by design.

When you see these: **do not conclude "broken."** Query the running system (fabric `health` / `loop/pending` / `council_query`). The surface advances when an agent or operator gives it input.

---

## Why it matters

- **The fabric is the eyes of the inference.** A file read or `curl` is a *slice*; the disk is not the system. The fabric reports its own live state — ask it first. (Curl said "wedged"; the fabric said "alive, 19 decisions waiting" in one call.)
- **`freeze ≠ broken`** (slice-engine law). A held / frozen surface is the *safe* state, not a failure; it materializes/advances on spawner-emit or operator crank (`E ≠ 0`), which is **design-law, not deflation**.
- **It fits the living frame.** Asolaria is not "frozen slices" — slices are the frozen-potential layer; the **matrix/fabric + engines + live agents** animate them. A decision-lane waiting for input is exactly that frame in miniature: potential, held safe, alive when inhabited.

---

## One-line form

> Don't call a held-safe, input-driven decision-lane "wedged." Ask the fabric. `freeze ≠ broken`.

*Carve-out clean: rule + measured service state only; no keys, secrets, PII, or envelope internals.*
