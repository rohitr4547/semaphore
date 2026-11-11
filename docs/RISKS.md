# Semaphore: Risk Profile and Initial Risk Register

**Status: Initial (G1).**

## Six-dimension Risk Profile

| Dimension | Rating | Evidence | Mitigation (for High) |
|---|---|---|---|
| IP / Concept | Medium | Original concept; references are attributed and no assets are reused (GDD §12). Unproven: whether yielding feels like cooperation rather than waiting. | – |
| Business / Audience | Medium | Co-op puzzle players are a clear group, but I have no player evidence yet. Classmates and friends can playtest in a browser. | – |
| Technology | Medium | Proven in my midterm: model calls to `gemini-3.5-flash-lite`, a bounded player interface, timeouts and failed-call accounting. I have built browser games before (Slot Racer). Unverified: calling the model from a Cloudflare Worker. | – |
| Team | Medium | Agent roles reuse artifacts that worked in my midterm (implementation agent, rule-change skill, read-only reviewer). New: Art Lead and release skills. I work alone, so my own capacity is the limit. | – |
| Genre / Mechanics | **High** | The central mechanic (simultaneous commit on one shared crossing, four signals) is untested. A fixed alternation might solve every shift, which would make signals pointless. | Paper prototype (task 1) and a solver comparison of alternation vs best play (task 3) before building the client; change traffic rules if alternation wins. |
| Timeline | **High** | A first playable with a working agent and an external playtest is due at the combined G2/G3 review, with Thanksgiving in between. | Build in the order engine → scripted partner → agent → client; greybox before polish; the cut list in GDD §10. |

**Do the Highs share a cause?** Yes. Both depend on the same unproven core loop: if the shared-crossing decision is not interesting, the time spent on everything else is wasted. That is why the first two tasks test the loop on paper and with a solver before any client work.

## Initial Risk Register

| Risk | Evidence or trigger | Impact / likelihood | Role | Mitigation | Contingency | Next check |
|---|---|---|---|---|---|---|
| If a fixed alternation solves every schedule by task 3, signals have no purpose | Solver shows no gap between alternation and best play | High / Medium | Design Lead | Schedules with deadline pressure and blocked sidings | Add a second shared resource (a passing loop) or asymmetric deadlines | Task 3 |
| If the model partner leaks or asks for hidden information | Adversarial test, trace review | Medium / Low | QA | `viewFor` is the only path to the agent; test that the hidden queue is absent | – | Task 6 |
| If the Worker cannot call the model by task 5 | Spike fails | Medium / Medium | Tech Lead | Try both authentication routes | Run the model partner from the Node harness only; the web build uses the scripted partner | Task 5 |
| If model latency makes rounds feel slow | Added waiting > 3 s median in the first build | Medium / Low | Tech Lead | Start calls when each phase opens | Shorter prompt; fewer history rounds | Task 7 |
| If I cannot recruit an external playtester before G2 | No session booked by Nov 30 | High / Low | Producer | Ask classmates at the G1 review | Remote session over screen share | Nov 30 |
| If the model partner performs no better than the scripted one | Automated comparison | Low for the project / Medium | BI | Report it as a result | The game ships with whichever partner players prefer | Task 8 |
