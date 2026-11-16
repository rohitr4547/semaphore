# Semaphore: Initial Project Plan

AI use: Claude and ChatGPT helped me explore concepts and draft the initial documents. ChatGPT helped review this revision for clarity, consistency and rubric coverage. I remain responsible for the design, evidence and final submission.

**Gate 1, 15 November 2026.** I own all tasks and decisions. Agents support the roles in CONFIGURATION.md. This is a proposal for prototype work, not a record of completed tasks. After G1 I will move owners, dependencies and criteria into Asana.

## Gate calendar

Dates come from the course syllabus (revised November 10, 2026). The Finals slot is assigned later.

| Gate | Date | Evidence / status |
|---|---|---|
| G1 recording and post | Nov 15, 11:59 p.m. ET | Recording, deck and initial documents. |
| G1 class review | Nov 16 | Live if selected, otherwise recording. |
| Approve / Hold decision | By Nov 17 | Instructor comment on CourseWorks. |
| Two Braintrust replies | Nov 19, 11:59 p.m. ET | Syllabus date; CourseWorks lists Nov 18, so I aim for Nov 18. Posts with fewer than two replies. |
| Deployed model checkpoint | Nov 23 | Deployed request, isolated sessions, forced timeout. |
| G2 Prototype / G3 Production review | Nov 30, recording Nov 28 | First playable, external playtest, comparison, updated documents and scope lock. |
| Alpha / Beta | Dec 2 / Dec 4 | Integrated features / locked content. |
| G4 Final / Pre-Launch | Dec 7, recording Dec 5 | Gold build and final evidence. |
| G5 Closure | Dec 14 | AAR and playtest log. |
| Finals | Dec 17–23, slot assigned later | Retrospective, recorded walkthrough and live presentation. |

## First work through G2

I budget **30 focused hours** through Nov 27, plus a recording buffer on Nov 28. Calendar days are not full workdays. If these estimates slip, I reduce presentation scope before cutting the agent comparison or external playtest.

| Task / working date | Agent support | Depends on | Hours | Acceptance criterion |
|---|---|---|---:|---|
| 1. Paper handovers, Nov 16–18 | Design | G1 direction | 2 | Two people try 3–6 handovers. Record one real yield trade-off or revise the loop. |
| 2. Deployed call/session spike, Nov 17–20 | Tech / Release | Chosen API route | 3 | One deployed validated action, two isolated sessions, forced timeout and stale-reply test. No browser key. |
| 3. Headless engine, Nov 18–20 | Tech | 1 | 6 | Complete deterministic shift with crossing, receiving switch, backlog, score and 24-beat ending. |
| 4. Baseline and feasibility solver, Nov 20–21 | Tech / Design | 3 | 3 | Scripted dispatcher uses no hidden deadlines. Solver finds feasible completion routes. Alternation tested on development cases. |
| 5. Agent view, phase schemas and traces, Nov 20–22 | Tech / QA | 2, 3 | 3 | Hidden-state, invalid-output, failure and call-cap checks pass. |
| 6. Greybox browser slice, Nov 21–24 | Tech / Art | 3, 4 | 5 | A new player can finish a shift with preview, signals and readable outcomes. |
| 7. Freeze and compare, Nov 24–25 | BI / QA | 4, 5 | 2 | Freeze policies/prompts, then open reserved results. 50 model and 50 scripted shifts across both partner policies, within approved budget. |
| 8. External playtest, Nov 25–27 | Design / BI | 5, 6 | 2 | At least one external person tries both partners. Note actual behaviour before interpretations. |
| 9. Independent review and repairs, Nov 26–27 | QA / Tech | 3–8 | 2 | Read-only findings recorded, my responses explicit, no unresolved boundary blocker. |
| 10. Update docs and scope lock, Nov 27 | Producer | 7–9 | 2 | Updated GDD/configuration/risk register, actual budget, playtest log and next gate decision. |
| **Total** | | | **30** | Nov 28 remains the recording buffer. |

Task 2 is an early feasibility spike, not permission to start a large client before testing the loop. Tasks 2–5 support the planned Nov 23 checkpoint. If the loop fails, I revise the smallest rule set before extending the build. If deployment fails, I report it rather than substituting a scripted-only checkpoint.

## Minimum G2 evidence

One complete shift, both partner policies, a deployed validated model action, enforced information and timeout boundaries, an external playtest and a small reproducible comparison. A discrete board is acceptable. A polished animation without this evidence is not the priority.

## Decisions and recommendations

| Choice | Origin of recommendation | My position |
|---|---|---|
| Cooperative dispatcher rather than rival | Earlier Claude brainstorming | Selected for this pitch. |
| Four explained signals rather than invented codes | Earlier ChatGPT review | Selected. The challenge is interpreting priorities. |
| Preview before commitment | Midterm playtest finding | Selected. It reveals only local consequences. |
| Simultaneous commits and conflict-as-delay | Earlier ChatGPT review | Provisional until paper test. |
| Receiving switch and private-deadline-safe baseline | Current consistency review | Proposed clarification of the prototype. Test before scope lock. |
| TypeScript and server-held session | Initial Claude stack recommendation, current review | Proposed. Authentication spike decides final hosting. |
| Eight trains, 24 beats and score values | Earlier design review | Provisional. Solver and playtest check balance. |

I decide scope, rules, evidence claims and releases. Agents may recommend and implement within a task. The instructor makes the gate decision.
