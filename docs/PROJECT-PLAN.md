AI use: Claude and ChatGPT helped me brainstorm related games, draft and structure this document and revise its wording. The design decisions marked as mine are mine.

# Semaphore: Initial Project Plan

**Status: Initial (G1).** Moves into Asana after G1, where owners, acceptance criteria, dependencies and the full Risk Register follow at G2/G3.

## Gate dates

| Gate / deadline | Date | What I bring |
|---|---|---|
| G1 Concept: post | Sun Nov 15, 11:59 p.m. | Recording, deck, Initial GDD and Configuration Document, this plan |
| G1 review | Mon Nov 16 (in class) | Live presentation if selected |
| G1 decision | by Tue Nov 17 | Approve / Hold comment on CourseWorks |
| Two Braintrust replies | Thu Nov 19, 11:59 p.m. | Replies to two classmates' G1 posts |
| Checkpoint: deployed model call | Mon Nov 23 (in class or by recording) | A deployed model call, isolated session state and a forced-timeout fallback |
| G2 Prototype + G3 Production (combined review) | Mon Nov 30; recording Sat Nov 28 | First playable with agent and one external playtest, Initial Playtest Log, scope-lock plan, Update GDD/Configuration, updated Risk Register, three Braintrust notes |
| Alpha / Beta | Wed Dec 2 / Fri Dec 4 | All features integrated / content locked; external release playtest between Alpha and Gold |
| G4 Final / Pre-Launch | Mon Dec 7; Gold recording Sat Dec 5 | Final GDD and Configuration, harness, Gold build, Update Playtest Log |
| G5 Retrospective / Closure | Mon Dec 14 | Final game and AAR, Playtest Log |
| Finals | Dec 17–23 (slot TBD) | Retrospective, 8–10 minute walkthrough, live presentation |

## First tasks through G2

Owner is always me; the delegated role is the agent doing the work.

| # | Task | Delegated role | Depends on | Estimate | Acceptance criteria |
|---|---|---|---|---|---|
| 1 | Paper prototype: two people play 3 rounds of one schedule with cards for trains and signals | Design Lead (prepare), me (run) | – | 0.5 day | I can say whether holding a train was ever a real choice |
| 2 | Engine core: board, trains, schedules, beats, interlock, scoring | Tech Lead | 1 | 1.5 days | Deterministic tests pass; a scripted shift runs headless |
| 3 | Solver check of schedules: compare fixed alternation with best play | Design Lead / Tech Lead | 2 | 0.5 day | Each evaluation schedule has a gap between alternation and best play, or it is changed |
| 4 | Scripted partner and two fixed player policies | Tech Lead | 2 | 0.5 day | Baseline rules in CONFIGURATION §1 implemented and tested |
| 5 | Worker spike: one model call from a Cloudflare Worker (due for the Nov 23 checkpoint) | Tech Lead, Publishing | – | 0.5 day | One validated `commit` returns in under 6 s; no key in the browser; isolated state per session |
| 6 | Agent interface, `viewFor`, safety hold, traces | Tech Lead | 2, 5 | 1 day | Tests: hidden queue absent from the view; invalid reply → safety hold; timeout → safety hold |
| 7 | Greybox client: canvas board, signal buttons, route preview, hold/depart | Tech Lead, Art Lead | 2 | 2 days | A person can finish a shift with the scripted partner |
| 8 | Freeze held-out schedules; run the automated comparison | BI, QA | 3, 4, 6 | 0.5 day | Report with deliveries, conflicts, safety holds, latency, cost; spend ≤ $2 |
| 9 | First external playtest (one shift with each partner, order alternated) | Design Lead, BI | 7, 8 | 0.5 day | Playtest Log entry: observations before interpretations |
| 10 | Independent review of the engine and agent boundary | QA (read-only reviewer) | 6 | 0.5 day | `REVIEW.md` with my response to each finding |
| 11 | Update GDD, Configuration, Risk Register; move the plan into Asana | Producer | all | 0.5 day | Documents labelled Update; Asana tasks have owners and criteria |

**Order of work:** 1 → 2 → 3 decides whether the core loop is worth building on (see the Risk Profile). If task 3 shows that alternation always wins, I change the traffic rules before building the client.

**Capacity:** about 9 working days of tasks between Nov 17 and Nov 27, with the Thanksgiving week (Nov 23–29) at half capacity. Tasks 2, 5 and 6 must be done for the Nov 23 checkpoint (deployed model call and forced-timeout fallback). Buffer: Nov 27–28, before the G2/G3 recording.

## Decisions so far: mine and my agents'

| Decision | Recommended by | Decided by |
|---|---|---|
| Make the agent a partner, not a rival | Claude (brainstorm) | Me |
| Four signals with explained meanings (not invented codes) | ChatGPT (review) | Me |
| Route preview before committing | My midterm playtest note | Me |
| Simultaneous commit with an interlock (no crashes) | ChatGPT (review) | Proposed; I decide after the paper prototype |
| TypeScript engine, Cloudflare Worker for model calls | Claude | Proposed; I decide before task 2 |
| Board size, 8 trains, 24 beats, scoring numbers | ChatGPT (review) | Proposed; tuned after playtests |
