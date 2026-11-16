# Semaphore: Initial Risk Profile

AI use: Claude and ChatGPT helped me explore concepts and draft the initial documents. ChatGPT helped review this revision for clarity, consistency and rubric coverage. I remain responsible for the design, evidence and final submission.

**Gate 1, 15 November 2026.** These are my current assessments. No Semaphore playtest or deployment measurement exists yet.

## Six dimensions

| Dimension | Rating | Evidence and uncertainty | Mitigation / next check |
|---|---|---|---|
| IP / Concept | Medium | The setting and proposed layouts are original. Commercial references are named, but the shared-crossing experience is untested. No third-party assets/code included. | Keep provenance in SOURCES.md. Review any later reuse before copying. Paper test checks concept. |
| Business / Audience | Medium | Co-op puzzle players are the intended audience. No evidence yet that they enjoy negotiating with this partner. No commercial revenue assumption. | Recruit external testers, ask for a concrete helpful/confusing handover. |
| Technology | Medium | I have prior model-call and browser-game experience. Server authentication, state isolation and latency in this stack are unverified. | Deployed spike by Nov 20. Escalate to High if it fails. A local-only model does not close the checkpoint. |
| Team | Medium | I own delivery, supported by coding/review agents. Art and release instructions are new. My time remains the bottleneck. | One task at a time, independently reviewed changes, limited daily commitments. |
| Genre / Mechanics | **High** | A fixed alternating dispatcher may perform as well as negotiation. Receiving duties could create confusion rather than choice. | Paper test by Nov 18, then solver/alternation comparison. Simplify traffic or receiving rules before adding features. |
| Timeline | **High** | Deployed checkpoint Nov 23 and proposed G2/G3 recording Nov 28 leave little time, with reduced Thanksgiving capacity. Current task estimates exceed a safe solo allowance. | Deliver a thin vertical slice first, budget 30 focused hours through Nov 27, leave Nov 28 for recording. Cut animation/sound/statistics before evaluation. |

**Shared cause:** the two High ratings overlap because the core coordination loop is still unproven. Timeline also has independent causes: deployment uncertainty and limited solo capacity. Early mechanics tests reduce both Highs, but do not remove the deployment dependency.

## Initial register

| Risk and trigger | Owner | Mitigation | Contingency / checkpoint |
|---|---|---|---|
| Alternation matches the full-information reference on every development schedule | Me, Design | Test contrasting arrival/deadline patterns and receiving pressure. Compare fairly without seeking model wins. | If no trade-off emerges, simplify and re-pitch a Hold. Do not add a second resource before G2. Nov 18–20. |
| Deployed authenticated request or session isolation fails | Me, Tech | Spike before full client. Test two concurrent sessions and a forced timeout. | Time-box Worker route, consider Node deployment if course permits. Unresolved by Nov 21 means escalation/Hold. |
| API waiting interrupts planning | Me, Tech | Start each call when its phase opens. Bound history and discard stale replies. | Target p95 ≤3 s/call and ≤2 s median extra wait. If missed, simplify prompts/UI or reduce beats after feasibility checks. |
| Player cannot interpret a hold or request | Me, Design | Fixed signal meanings, event log, visible preview and tutorial. | Paper/greybox playtest before polish. Remove ambiguous cues rather than invent explanations. |
| No external tester booked by Nov 24 | Me, Producer | Invite classmates at the G1 review. | Remote screen-share test on Nov 25–27. Disclose if it cannot happen. |
| Model does not improve results or experience | Me, BI | Competent baseline, paired schedules and honest reporting. | Keep the model evidence for the course. Choose the preferred shipping partner after reporting the result. |
| Prompt/output usage exceeds the estimate | Me, Tech | Token caps and request-cost reservation. Keep unknown timeout usage separate. | $3 working target, stop new paid calls at $5 threshold, disclose billing uncertainty. |
| Hidden data leaks through view, legal list, error or trace | Me, QA | State-difference boundary tests, allowlists and server authority. | Block live play until fixed. Re-run affected evidence. |

The full Risk Register moves into the production plan after G1. Ratings change with evidence, not because a task is marked complete.
