# Semaphore: Initial Configuration Document

AI use: Claude and ChatGPT helped me explore concepts and draft the initial documents. ChatGPT helped review this revision for clarity, consistency and rubric coverage. I remain responsible for the design, evidence and final submission.

**Gate 1 concept, 15 November 2026.** All runtime components, paths and tests below are planned. This is a configuration specification, not evidence of a deployed agent. I have selected the observation boundary and comparison design. Mechanics and stack remain provisional until the first feasibility checks.

## 1. Runtime contract

| Item | Planned configuration |
|---|---|
| Job | Control the partner's signal, local receiving switch and hold-or-depart decision. |
| Model | `gemini-3.5-flash-lite` through the global Vertex AI endpoint. Verify model identifier, access and deployed authentication in the spike. |
| Phase 1 | `send_signal(signal)`. Context contains the agent's local state and past public history. It contains neither side's unrevealed current signal. |
| Phase 2 | `commit(train_id_or_null, receiving_siding, hold_or_depart)`. Context additionally contains both revealed signals. It never contains the human's current commit. |
| Memory | Last eight resolved rounds of role-filtered history. No cross-shift notes or learning in the first playable. |
| Model settings | Provider default temperature, structured JSON action. Start with minimal/disabled reasoning where supported, maximum 200 total billed output tokens per call. Verify actual response and reasoning usage in the spike. |
| Prompts | Versioned rules, public signal meanings, shared-score goal and schema. Planned path: `agent/prompts/dispatcher-v1.md`. No scenario solution or privileged strategy hints. |
| Planned executable artifact | `engine/agent-interface.ts` plus `agent/manifest.json` and the deployed request handler. These files do not exist at G1. |

### Observation boundary

`viewFor(side, phase)` returns local queues and current backlog count, local train destinations and deadlines, local receiving occupancy, the public crossing (including the marker on an already-departed train), revealed signals, beat and shared score. It includes only resolved public actions from history, not either side's historic private queues or deadlines. The current opposite signal is absent until the reveal phase.

The model receives no human queue/deadlines, future arrivals, schedule, seed, hidden commits, source files, shell, solver or evaluator traces. Developer traces may include full state for offline verification, but those traces never enter a player's view. Legal actions derive from the role's own state. An invalid-action message must not reveal hidden occupancy or hidden objects.

### Validation and failure policy

The engine checks schema, phase, session, beat and action ownership before changing state. The interlock resolves occupancy and simultaneous requests after both commits. A blocked but legal departure counts as a conflict or wait, not an invalid model action.

A failed signal call produces a neutral “no signal / call failed” indicator. It does not invent a request or Urgent. A failed commit produces a labelled safety hold, preserving the previous receiving switch. Invalid human input stays in the planning phase with an actionable error. Failed model actions count in call/failure totals. No retry or hidden model substitution is allowed within that phase.

Timeout: **6 seconds per call**, with cancellation where supported and late responses discarded using session/beat/phase identifiers. At most **48 call attempts per shift** and **144 per three-shift session**. There are exactly two possible calls per beat over 24 beats. Once a cap is reached, stop requesting and use labelled safety holds. The shift still ends at beat 24. Pause and resume must not replay a charged call.

The player can pause or exit. Swapping the partner policy is a developer-only diagnostic and marks the trace as mixed-policy, excluded from the primary comparison.

## 2. Scripted dispatcher

The baseline gets the same filtered view and uses the same action schema. It can inspect only its own deadlines.

1. Choose the local earliest-deadline waiting train. Signal Please wait if the receiving berth is blocked, otherwise Urgent if the train is within three beats of its deadline, Request crossing if a train is waiting, otherwise Ready to receive.
2. Set the local receiving switch to match a visible incoming train. Otherwise keep its previous valid setting.
3. Hold when the crossing is occupied or no train is ready. Respect Please wait. Yield to an opposing Urgent unless its own signal is also Urgent.
4. With two Requests or two Urgents, use a **public alternating priority**, based on the last departure side. Start with left priority. Both dispatchers can reconstruct this without seeing the other queue.
5. Depart when permitted by these rules. If both nevertheless depart, apply the same engine conflict rule as for the model.

The policy is responsive but fixed. The experiment asks whether contextual model decisions add value beyond those explicit rules. It does not claim that scripted partners cannot coordinate.

## 3. Budgets and basis

Prices checked **15 November 2026** on Google's Standard, global pricing table: **$0.30 input and $2.50 output per million tokens** for Gemini 3.5 Flash-Lite. Source: https://cloud.google.com/model-garden/pricing . No caching, batch or priority discount assumed.

| Quantity | Planning value | Basis and limit |
|---|---|---|
| Calls per shift / session | 48 / 144 | 2 phases × 24 beats, 3 shifts. Attempts include failures. |
| Typical tokens/call | 1,500 input, 60 output | Estimate: rules 600, view 300, history 500, schema 100. No Semaphore measurement yet. |
| Typical shift / session | $0.0288 / $0.0864 | `48 × (1500 × 0.30 + 60 × 2.50) / 1e6`. |
| Output-cap scenario | $0.0456 / $0.1368 | Same estimated input, 200 billed output tokens per call. This is conditional, not an absolute cost bound. |
| Three-times-input sensitivity | $0.072 / $0.216 | 4,500 input and 60 output tokens per call. |
| Latency basis | Prior midterm means 1.40–1.72 s/call | Self-reported in supplied midterm documents. Not a Semaphore benchmark and not a median. |
| Latency target | p95 ≤ 3 s/call, 6 s timeout | Measure at least 20 deployed calls. Target extra wait after human input: ≤ 2 s median. |
| Worst additional round wait | Up to 12 s | Two sequential 6 s phase timeouts. At 1.5 s per call, 48 serial calls represent about 72 s of model time before overlap. |
| Automated comparison | 50 model shifts, about $1.44 typical | 5 schedules × 2 fixed partner policies × 5 repetitions. 50 paired scripted shifts cost no model tokens. |
| G2 working budget | $3 target, $5 stop threshold | Allows the comparison plus limited spikes/playtests. Reserve estimated request cost before dispatch, cap prompt length, include unknown usage conservatively. Provider billing can lag. |

The main uncertainties are prompt growth, billed reasoning/output tokens, cold starts and provider latency. Record actual usage and reprice before the first benchmark. Hosting, tax and unknown charges on client-abandoned calls are outside the token estimate and must be disclosed separately.

## 4. Architecture and deployment

**Proposed:** a pure TypeScript engine, Vite canvas client, Node harness and a Cloudflare Worker backend. The published client sends action requests and receives a role-filtered view. The server owns the authoritative session. The client must not be the authority for the model's observation.

Use isolated server session storage, proposed as a Durable Object per shift, with unique request IDs and bounded retained traces. Keep Vertex credentials in server secrets. A Google AI Studio key is a different API/authentication path and must not be described as Vertex authentication. Prove one chosen route in the deployed spike before committing to it.

The fallback is a small Node server with server-held credentials and the same interface, subject to the course's deployment requirements. A scripted-only browser build can test mechanics, but **does not satisfy the deployed model checkpoint**. If deployment remains blocked, report the gap and request a Hold or agreed recovery plan rather than calling a local-only demo complete.

## 5. Evaluation and verification

Declare three development schedules and two reserved schedules. Build them and validate feasibility before tuning. Freeze prompt, policy, schema and code versions before opening held-out results. Do not design or revise a schedule because it makes the model look better. A repeat after changes is a disclosed development run, not a fresh held-out test.

Automated comparison pairs both candidate dispatchers with the same two fixed partner policies: a signal-responsive dispatcher and an assertive dispatcher. Each condition uses the same schedules/seeds. Five repetitions measure live model variation; provider sampling may be unseeded. Report each policy's results separately. Full-information solver results are feasibility references, never a fair-information competitor.

Record correct/on-time/late/wrong deliveries, total score, conflicts, public yield events, invalid actions, safety holds, failed/timeout calls, latency distributions, tokens and estimated cost. Preserve incomplete episodes and explain exclusions. A signal followed by a hold is association, not proof of intent or causation. If feasible, add a controlled no-signal ablation after the primary comparison.

Recruit at least one external tester before G2, aiming for three. Each tries one shift per partner on matched scenarios, with order and schedule assignment alternated when there is more than one tester. One tester gives formative observations, not a preference estimate. Ask the playtest question in GDD §6, collect specific moments and keep identities private.

### Planned deterministic checks

Rule and scoring invariants, the two-beat crossing, contested departures, berth release order, overflow, successful completion and beat termination. Boundary tests compare states that differ only in the other queue, hidden schedule or commit and require identical agent views. Adversarial checks cover invalid ownership, malformed JSON, phase mismatch, timeout, stale responses, cross-session access and duplicate commits. CI runs these without provider keys. Planned commands and workflow paths are specifications until implementation exists.

### Trace and manifest

Planned `agent/manifest.json` records purpose, tools, prohibited actions, context sources, memory policy, model/settings, prompt hash, failure cases, logging, pause/override points, test command and CI workflow path (`.github/workflows/ci.yml`). Log exact role-filtered inputs in private evaluation evidence, outputs, validation, outcome, timing and usage. A hash alone cannot reproduce an action. Export only anonymised, credential-free traces.

## 6. Agent team and human responsibility

I act as Executive Producer and Creative Director. These are **planned responsibilities**, not a team of people or already-executed agents. I approve scope, rules, conclusions and releases. Agents prepare recommendations and evidence.

| Role | Planned artifact and context | Permissions and output | Check and my decision |
|---|---|---|---|
| Producer | `.claude/skills/work-cycle/SKILL.md`, plan/risk log | Read repo, write task and PPP drafts | Dependencies/criteria inspectable. I accept commitments. |
| Design Lead | `.claude/skills/rule-change/SKILL.md`, GDD and playtests | Propose rules, failing tests and schedules | Solver and playtest evidence. I approve rules. |
| Tech Lead | `CLAUDE.md`, scoped implementation session | Edit engine/client/server, run offline tests. No credentials or autonomous paid calls. | QA review and CI. I accept code. |
| Art Lead | `docs/art-style.md` and art-review task | Original track/signal assets and screenshots | Colour-plus-shape and readable previews. I accept presentation. |
| Narrative / Audio | Design Lead task, tutorial and asset list | Draft onboarding, propose optional licensed sound | Comprehension and asset provenance. I approve text/assets. |
| QA | Separate read-only `.claude/agents/rules-reviewer.md` | Inspect changes and tests, write findings, no instruction to approve | `REVIEW.md` responses. I decide each finding. |
| BI | Planned `tools/evaluate.ts`, anonymised traces | Run approved budgeted comparisons, draft report | Recompute summaries from traces. I interpret findings. |
| Publishing / Release | Proposed `wrangler.toml`, release checklist | Prepare build and deployment steps | Secrets/session checks and fresh-browser test. I authorise deployment. |

Confirm each artifact in the first implementation cycle. Retain named session links and decisions in the eventual agent-team log. The manifest keeps every field the syllabus lists for the extension artifact.
