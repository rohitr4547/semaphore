# Semaphore: Initial Game Design Document

AI use: Claude and ChatGPT helped me explore concepts and draft the initial documents. ChatGPT helped review this revision for clarity, consistency and rubric coverage. I remain responsible for the design, evidence and final submission.

**Gate 1 concept, 15 November 2026.** This document specifies a prototype. There is no playable build or Semaphore performance evidence yet. **Selected** identifies the direction I have chosen. **Provisional** identifies mechanics to test. **Open** identifies a question with an owner and a check.

## 1. The X

Semaphore is an animated co-op game where you and an AI partner dispatch trains across one shared track, and you feel the satisfaction of getting a difficult handover right.

## 2. Player and motivation

I am designing for people who enjoy short cooperative puzzles, playing on a laptop browser. Fellowship is the main motivation, with mastery coming from making better dispatch decisions. A shift should take roughly 4–8 minutes, subject to testing. Three shifts form an optional longer session.

My observable hypothesis is that a player will voluntarily hold a train after seeing the partner's urgent signal, then explain the trade-off afterwards. I also want to see whether the player changes their next signal after a misunderstanding. These are playtest hypotheses, not evidence that the experience already works.

## 3. Pillars and alternatives

| Pillar | Design consequence |
|---|---|
| Cooperation has a visible cost | Yielding uses a beat that could have served the player's own queue. |
| Consequences are readable | Preview the local route and show resolved movement, deadlines and score changes. |
| Planning sets the pace | Advance in beats after both decisions. Model speed does not control train physics. |

I considered **The Other Adventurer**, a text race involving a rival's notebook, and **Pit Wall**, a racing game with an AI crew chief. The former kept the project too close to my text-adventure midterm. The latter added continuous-control and timing work before I could test the crew chief's value. Semaphore gives me an animated game and one bounded coordination question. A third idea, Market Whispers, repeated too much of the financial setting I had already explored.

## 4. Core loop

1. **Act:** inspect local trains and deadlines, then choose one of four signals. Both sides submit signals independently.
2. **Respond:** reveal both signals together.
3. **Interpret:** decide whether the partner needs the crossing more urgently. Select a train, preview the receiving route and commit hold or depart. Commits remain hidden until resolution.
4. **Respond:** validate both actions, advance one beat and animate movement or waiting.
5. **Next action:** read the outcome and score changes, then decide what to signal next.

Example: my next train still has five beats of slack. The partner signals Urgent. I can yield now, but I cannot inspect its queue to verify that deadline. I watch the resolved crossing and use that public history when deciding the next handover. This illustrates the intended choice, not a recorded playtest.

## 5. First playable rules

These are **provisional prototype rules**. They provide a complete loop for testing, not a claim that balance is settled.

### Board, dispatch and receiving

- One board, one bidirectional shared section and two dispatchers. Each side has two waiting positions, one receiving berth and a switch selecting one of two destination sidings.
- Every train must cross to the opposite side. Its origin dispatcher chooses which waiting train to release. The receiving dispatcher sets its own local switch. This separates the sending and receiving jobs.
- Destination uses a colour **and** a shape. A train's destination marker becomes public on entering the shared section, giving the receiver a chance to align its switch.
- A departure occupies the shared section for two simulation beats. A switch committed during those beats determines the receiving route when the train exits. A berth holds a delivered train for one additional beat, then clears automatically.
- If both sides request departure onto an empty section in the same beat, neither enters. An occupied section also blocks departure. These are legal but unsuccessful departures, not schema errors.
- If the receiving berth is occupied at the exit beat, the train remains on the shared section and the crossing stays locked. The berth can clear on a later beat. The engine never silently reroutes a train.
- At each beat, the engine resolves existing berth clearances, existing crossings and deliveries, then admissible new departures. New crossings cannot finish in their departure beat. A newly filled berth cannot clear in that beat.
- Arrivals use a fixed schedule. If both local waiting positions are full, the arrival remains in an off-board backlog and its deadline continues to approach. The current backlog count is local information. Future arrivals remain hidden.

### Signals

| Signal | Explained meaning |
|---|---|
| Request crossing | I want to send a train across. |
| Ready to receive | I can receive a train. |
| Please wait | Please hold your departure this beat. |
| Urgent | My next train is close to its deadline. |

The signal meanings are fixed. Signals advise the other side but do not enforce reservations or guarantee that it will yield. Each round allows one signal. Their shapes and text labels remain readable without colour.

### Ending and score

Eight trains, four originating on each side, arrive over a shift of at most 24 beats. Finish early only when all eight trains have resolved. A correct delivery by its deadline earns +3, a late correct delivery +1, and a wrong siding −2. A wrong delivery leaves play and does not count as correct. Unresolved trains earn 0 when the shift ends.

The initial completion target is six correct deliveries. Score, punctuality and completion are reported separately so six late deliveries are not described as optimal play. A full-information solver will check feasibility before an arrival schedule enters evaluation.

## 6. The partner and the comparison

The model controls one side's signal, departure and receiving switch. It sees its local trains and deadlines, the public crossing, revealed signals and the last eight rounds of public outcomes. It cannot read the human's queue, deadlines, uncommitted actions, future schedule, seed or source files.

I compare it with a competent scripted dispatcher that uses the same observation and actions. It prioritises its own earliest deadline, routes visible incoming trains correctly, reacts to signals and uses public tie-breaking when both sides want the crossing. It never compares two private deadlines.

The hypothesis is that recent interaction history helps the model adjust its yielding and requests in a way the player can understand. A fixed baseline still responds to signals. What it lacks is the ability to revise its dispatch policy from the current exchange. Whether that makes a meaningful difference is the experiment.

I will alternate partner order in a small within-player playtest and ask: **When you both wanted the crossing, what did you think your partner would do, what happened, and what did you change next?** I will also ask players to identify a specific helpful or confusing handover. Observations come before my interpretation.

## 7. Controls and feedback

Click a local waiting train, select a receiving siding and commit Hold or Depart. Use buttons or keys 1–4 to signal. The preview shows only local routing consequences and travel-time assumptions. It cannot predict hidden commitments or promise an on-time arrival.

The screen shows the current beat, score changes, crossing occupancy, both signals and resolved public actions. Feedback describes events, such as “Partner held. Your train departed.” It does not claim to know why a model acted. A model explanation is not ground truth about its reasoning.

My midterm playtester entered an irreversible ending without understanding the points left behind. I carried that lesson into route previews and visible score changes. I have not yet run a Semaphore playtest.

## 8. Scope and cuts

**Included by G2/G3:** one junction, four signals, the complete shift, route preview, a scripted partner, a deployed model partner, traces, five schedules and an external playtest. A flat 2D browser presentation is sufficient.

**Excluded:** human networking, procedural maps, a campaign, free-text or voice chat, cross-shift learning, mobile optimisation and music. No commercial game assets are planned.

**Cut first:** smooth movement in favour of discrete movement, optional sound, extra end-screen charts, then eight trains down to six with retested schedules. Keep the score summary, knowledge boundary, failure handling, baseline and evaluation. Do not add another shared resource to rescue an unproven loop before G2.

## 9. Open questions

| Question | Owner and next evidence |
|---|---|
| Does yielding create a meaningful choice? | Me, Design Lead support: paper test and alternation comparison by Nov 18. |
| Can receiving routes add useful decisions without confusion? | Me: first greybox playtest. Simplify receiving if necessary. |
| Are eight trains and 24 beats feasible and readable? | Tech Lead solver check, then human shift timing. |
| Does the model improve coordination or only produce variation? | BI comparison and external playtests before G2/G3. |
| Can a browser session use the deployed model safely? | Tech Lead authentication/session spike by Nov 20. |

## 10. References and attribution

I use these as design references. No referenced commercial art, maps, dialogue, code or branding is included. Specific links are in [SOURCES.md](SOURCES.md).

| Reference | Specific mechanic or moment | Borrowed principle and deliberate change | Reuse position |
|---|---|---|---|
| Hanabi, Antoine Bauza | Restricted hints communicate under imperfect information | Four explained signals. No cards or hint-token economy. | Reference only. No game materials copied. |
| Keep Talking and Nobody Explodes, Steel Crate Games | The defuser sees the bomb while experts consult the manual | Split observations. Both dispatchers also act on their own side. | Commercial reference, no assets/code copied. |
| Mini Metro, Dinosaur Polo Club | Station shapes and simple lines make routes legible | Original track layout with colour-and-shape destinations. Fixed junction rather than network construction. | Commercial reference, no visual assets copied. |
| Overcooked, Ghost Town Games | Partners share bottlenecks and divide tasks | Shared crossing and receiving duty. Planning beats replace real-time movement. | Commercial reference, no assets/code copied. |
| Bard et al., The Hanabi Challenge | Cooperative play with imperfect information | Motivates the coordination question. No claim of human-like understanding. | Research citation, no code reused. |
| Carroll et al., NeurIPS 2019 | Self-play coordination does not guarantee human compatibility | Test with humans and a competent baseline. | Research citation. Overcooked-AI code is not reused. |
| My Zork midterm | Bounded observations, logged failures and frozen evaluation | Reuse the approach. Inspect any adapter's actual license before copying code. | No code copied at G1. Preserve required notices if later reused. |

Sound is optional and not sourced yet. If a CC0 pack is added, record the exact pack URL, license and included files before release. Semaphore's conflict rule is a simplified game design inspired by shared railway sections, not a realistic railway safety model.
