# Semaphore: Configuration Document

**Status: Initial (G1).** Marked **[decided]**, **[proposed]** or **[open]** as in the GDD. Nothing here is implemented. Executable artifacts listed below are planned paths; they do not exist yet.

## 1. In-game agent: the partner dispatcher

| Field | Specification |
|---|---|
| Purpose | Dispatch the partner's side of the junction: one signal, then one route and hold-or-depart, per round. **[decided]** |
| Interface (executable artifact) | A function-calling interface with two tools, `send_signal(signal)` and `commit(train_id, siding, hold_or_depart)`, defined in `engine/agent-interface.ts` with a JSON schema. This is the project's required executable extension artifact. **[proposed]** |
| Context it receives | Its own side: trains in its waiting positions, their destinations and deadlines (beats left), its sidings' occupancy; the public crossing state (free / occupied, by whose train, beats remaining); both signals this round; the last 8 rounds of public history (signals, resolved moves, deliveries); the beat number and the score. **[decided]** |
| Knowledge boundary | Never receives: the other side's queue or deadlines, future arrivals, the schedule, the seed, the engine state object, other players' traces or any source file. The agent's view is built by one function, `viewFor(side)`, the only path from engine to agent. **[decided]** |
| Actions | Exactly the tools above. The engine rejects any train the side does not hold, any siding not reachable, any value outside the schema. **[decided]** |
| Memory | The bounded in-shift history above. No memory across shifts in the first playable. **[decided]** |
| Model and settings | `gemini-3.5-flash-lite`, provider default temperature, at most 200 output tokens, JSON-only reply. **[proposed]** (Basis: used for the Zork midterm, 1.4–1.7 s per call measured there.) |
| System instructions | States the goal (shared score), the four signals and their meanings, the interlock rule and the reply format. Contains no strategy hints that the scripted partner does not also encode. Versioned in `agent/prompts/dispatcher-v1.md`. **[proposed]** |
| Validation | The engine validates every action. An invalid action, an unparseable reply or a call failure becomes a **safety hold** (no departure; no signal, shown as "—"), clearly labelled on screen and in the trace, and counted separately from the agent's real decisions. Never a substitute move. **[decided]** |
| Timeout / retry / spend | Timeout 6 s per call; no retry within a round; at most 60 calls per shift and 200 per session; a session stops calling the model once its cap is reached, and the shift continues with safety holds. **[proposed]** |
| Logging | Each call: round, phase, prompt version, the agent's view (hashed if long), reply, validated action or rejection reason, latency, tokens. One trace file per shift. **[proposed]** |
| Human override | The player can pause. A developer flag swaps the partner for the scripted dispatcher mid-session (used for playtests, logged). **[proposed]** |
| Failure modes to test | Asks for or hallucinates hidden information; departs into a contested crossing every beat; ignores *Urgent*; replies with prose; slow replies. Each becomes a test case. **[proposed]** |
| Evaluation | See §3. **[proposed]** |

### Scripted baseline (same view, same tools) **[decided]**

1. Signal *Urgent* if its earliest train has ≤ 3 beats to its deadline; otherwise *Request crossing* if that train must cross; otherwise *Ready to receive*.
2. Commit its earliest-deadline train. Depart if the crossing is free and the partner did not signal *Urgent* or *Please wait*; otherwise hold.
3. Simultaneous requests with no *Urgent*: the side with the earlier deadline goes; on a tie, the left side goes.

The baseline must be competent: if it matches the model, that is a real result, not a failure of the project.

## 2. Budgets

| Budget | Number | Basis |
|---|---|---|
| Calls per shift | 48 (2 per round × 24 rounds) | Signal phase and commit phase each need one call. |
| Input tokens per call | ≈ 1,500 | Rules and format ≈ 600; state ≈ 300; 8 rounds of history ≈ 500; schema ≈ 100. To be measured on the first build. |
| Output tokens per call | ≈ 60 | One JSON action with a short reason. |
| **Cost per shift** | **≈ $0.03** | 48 × (1,500 × $0.30 + 60 × $2.50) / 1,000,000 = $0.029, at the Vertex AI Standard price for `gemini-3.5-flash-lite`, read 2026-11-04 (my midterm's `prices.json`). |
| **Cost per session** (3 shifts) | **≈ $0.09** | 3 × $0.029. |
| **Latency per call** | **≈ 1.5 s median, 6 s timeout** | Midterm measurements for the same model: 1.40–1.72 s mean per call. |
| Added waiting per round | ≈ 0 s median, ≤ 6 s worst | Each call starts when its phase opens and runs while the player is deciding (players take several seconds); the beat waits only if the agent is slower than the player. To be verified in the first build. |
| Evaluation spend before G2 | ≤ $2 | 5 schedules × 5 seeds × model partner ≈ 25 shifts × $0.03 ≈ $0.75, plus development and playtests. Hard stop at $5 of credits. |

The largest uncertainty in these numbers is the input size: if history makes prompts 3× larger, cost per session is still under $0.30.

## 3. Evaluation plan (proposed tests and baseline, G1 level)

- **Deterministic tests** (no model): rules and interlock, scoring, schedules, `viewFor` never containing the other side's queue, safety hold on invalid or failed calls, the round waiting for both commits.
- **Automated comparison:** model partner vs scripted partner, each paired with the same fixed "player" policies (an earliest-deadline player and a stubborn player who never yields), on 3 tuning and 2 **held-out** schedules × 5 seeds. Report deliveries, lateness, conflicting requests, yields, invalid actions, safety holds, latency, tokens and cost. Held-out schedules are frozen before any prompt tuning, as in my midterm.
- **Human playtests:** each player plays one shift with each partner, order alternated, without being told which is which. Questions: *When you both wanted the crossing, could you tell why your partner went or waited? Did it change what you signalled next?* Record observations before interpretations (reader §5.9).
- **Playtest question (G2):** When priorities conflict, can the player tell whether the partner yielded because of their signal, and does that change how they signal next?

## 4. Technology **[proposed, my decision pending]**

- **Engine:** pure TypeScript, deterministic and seedable, no DOM access, so the same code runs in the browser and in the Node test and evaluation harness.
- **Client:** HTML canvas, TypeScript, Vite.
- **Model calls:** a Cloudflare Worker (the course's taught publishing and AI-worker path) holds the credentials and forwards only the agent's view; the browser never sees a key.
- **Hosting:** Cloudflare Pages for the static client.
- **Open:** how the Worker authenticates to Vertex AI (a service-account token vs an AI Studio key). *Answer by:* a one-call spike in week 1. Fallback: the scripted partner only, with the model partner run from the Node harness.

## 5. Production agent team: role coverage (planned)

I am Executive Producer and Creative Director. Agents recommend; I decide; the instructor approves gates.

| Role | Executable artifact (planned) | Purpose | Context | Tools / permissions | Output → recipient | Evaluation | My approval boundary |
|---|---|---|---|---|---|---|---|
| Producer | Claude Code with `.claude/skills/work-cycle/SKILL.md` | Turn the plan into weekly tasks and PPP drafts | GDD, plan, risk register | Read repo; write `docs/plan/` | Task list and PPP → me | Tasks have acceptance criteria | I accept every commitment and scope change |
| Design Lead | `.claude/skills/rule-change/SKILL.md` (adapted from my midterm) | Specify rules, schedules, balance; propose playtest questions | GDD, playtest log | Read repo; write `docs/` | Rule changes with a failing test first → Tech Lead | Solver check on schedules | I approve every rule and schedule |
| Tech Lead | Implementation agent with `CLAUDE.md` | Engine, client, Worker, harness | Repo | Edit code, run tests; no credentials; live calls only when I run them | Build and test evidence → QA | CI green; tests listed in §3 | I merge |
| Art Lead | `.claude/skills/art-pass/SKILL.md` with `docs/art-style.md` | Readable shapes, colours (colour-blind safe), animation timing | Style guide, screenshots | Write `client/assets/` | SVG assets → Tech Lead | Readability check on screenshots | I accept the visual direction |
| Narrative and Audio | Covered by the Design Lead skill | One-screen tutorial; CC0 sound selection | GDD | Write tutorial text | Text and sound list → Tech Lead | Playtest comprehension | I approve the text |
| QA | `.claude/agents/rules-reviewer.md` (read-only, separate session) plus the test suite | Independent review; adversarial agent tests | Diffs, GDD, configuration | Read only | Findings → me | Findings answered in `REVIEW.md` | I decide each finding |
| BI | `tools/evaluate.ts` and a report-writing session | Run the comparison and summarise with limitations | Traces | Read traces; write `results/` | Report → me and Design Lead | Numbers reproduce from traces | I decide what the results mean |
| Publishing and Release | `wrangler.toml` and a release checklist skill | Deploy, write play instructions | Build | Deploy only when I run it | Public build and README → playtesters | A playtester can open and play | I approve each release |

**Open:** the Art Lead and release skills are new; the others adapt artifacts that worked in my midterm. I will confirm each one is usable during week 1 before relying on it (Team risk).
