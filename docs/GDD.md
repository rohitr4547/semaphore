AI use: Claude and ChatGPT helped me brainstorm related games, draft and structure this document and revise its wording. The design decisions marked as mine are mine.

# Semaphore: Game Design Document

**Status: Initial (G1).** Statements are marked **[decided]** (my decision as Creative Director), **[proposed]** (a recommendation I have not yet accepted) or **[open]** (an unresolved question, with how I plan to answer it). Nothing here is implemented yet.

## 1. The X

Semaphore is a small animated co-op game in which you and an AI partner run opposite sides of a rail junction, sharing one stretch of track and talking only through four signals, and you feel the satisfaction of getting a difficult handover right. **[decided]**

## 2. Player and motivation

- **Player:** people who enjoy short co-op puzzle games such as *Overcooked*, *Keep Talking and Nobody Explodes* or *Hanabi*, playing alone on a laptop browser in sessions of about ten minutes. **[decided]**
- **Motivation:** *Fellowship and social connection* first, *Challenge and mastery* second (Motivation Atlas groups; MDA's "fellowship" aesthetic, Hunicke et al. 2004). The dynamic I am relying on is interdependence: a goal that is "more difficult to achieve alone". **[decided]**
- **Observable hypothesis:** if the game works, a playtester will hold their own train, without being told to, so the partner's urgent train can cross, and afterwards can say why they did it. A second sign: the player changes which signal they send after seeing how the partner responded to the last one. **[decided as the hypothesis; untested]**

## 3. Core loop

One round, about 10–20 seconds of real time:

1. **Act (signal):** you see your trains, their destinations and deadlines, and send one signal. The partner sends one at the same time.
2. **Respond (reveal):** both signals appear on the signal board.
3. **Interpret and act (commit):** you pick a train, set its route (a preview highlights the path and travel time) and choose *hold* or *depart*. The partner commits too. Neither sees the other's commitment.
4. **Respond (advance):** the engine advances one beat and animates it: trains move, the crossing locks or frees, deliveries score.
5. **Next action:** you read what happened, including whether the partner yielded, and plan the next handover.

**[decided]**

## 4. Pillars

Used to choose between designs. **[decided]**

1. **Every handover is a decision.** If a choice about the shared track has an obvious answer every time, it is cut or changed.
2. **You can read your partner.** Signals and their consequences must be legible on screen. The player should always be able to see what the partner did and work out why.
3. **Calm, not frantic.** Beats wait for both dispatchers. The tension comes from deadlines and trade-offs, not reaction speed. A *Mini Metro*-like readable map, not *Overcooked* chaos.

## 5. Mechanics

### 5.1 Board (first playable) **[proposed]**

- One junction. Each side has one entry track, **two waiting positions** and **two destination sidings**. Both sides share **one crossing** in the middle.
- Each train has a destination siding (marked by colour **and** shape, for colour-blind readability) and a **deadline** in beats.
- A train going to the far side must cross the shared track. A crossing takes **two beats**.

### 5.2 Signals **[decided: four signals with explained meanings]**

| Signal | Meaning (shown in the tutorial and on hover) |
|---|---|
| Request crossing | I want to send a train across. |
| Ready to receive | My side can take a train now. |
| Please wait | Hold your train this beat. |
| Urgent | My next train is close to its deadline. |

The meanings are fixed and explained up front. The game is about *how you respond* to a signal, not guessing what a symbol means.

### 5.3 Simultaneous commit and the interlock **[proposed]**

- Both dispatchers commit without seeing the other's commitment.
- A **safety interlock** stops any train entering an occupied crossing. If both sides depart onto an empty crossing in the same beat, **neither moves** that beat. Poor coordination costs time, never an unavoidable crash.
- The **route preview** shows the path and expected travel time before you commit. It does not reveal the partner's trains or promise the crossing will be free. (From my midterm playtest: a player did not notice that an irreversible move cost them points, so consequences are shown before commitment.)

### 5.4 Shift, scoring, ending **[proposed; numbers to tune]**

- A shift has **8 trains** (4 per side) on a fixed arrival schedule and ends after **24 beats**.
- On-time delivery **+3**, late **+1**, wrong siding **−2**, undelivered **0**. Score is shared.
- Target: **6 correct deliveries** in a shift. **[open]** I will check with a solver that every evaluation schedule allows this before using it.
- End screen: deliveries, late deliveries, routing mistakes, beats lost to conflicting requests, and the beats where one side yielded.

## 6. Dynamics I expect, and how I will check

| Expected dynamic | Risk | Check |
|---|---|---|
| Players trade turns on the crossing according to urgency | A fixed alternation solves every shift | Solver comparison: alternation vs best play on each schedule. **[open]** |
| Signals change behaviour | Players ignore signals | Count signal-then-response pairs in traces; playtest interviews |
| Yielding feels like cooperation | It feels like waiting | Ask playtesters to name a moment the partner helped or hindered them |

## 7. The AI partner (summary; full specification in `CONFIGURATION.md`)

- **Job:** dispatch the other side: send one signal, then commit one route and hold-or-depart per round. **[decided]**
- **Knows:** its own trains, deadlines and sidings; the public crossing state; both signals; a bounded history of this shift. **Never:** my queue, future arrivals, the schedule or engine state. **[decided]**
- **Validated:** the engine checks every action. An illegal or unparseable action, or a timed-out call, becomes a labelled *safety hold* and is logged separately from the agent's real decisions. **[proposed]**
- **Scripted baseline:** a competent dispatcher. Earliest-deadline train first, checks the crossing, yields to *Urgent*, fixed tie-break on simultaneous requests. Same information, same actions. **[decided]**
- **What the player loses with the baseline:** a partner that reconsiders who should go first when the situation changes. After a few shifts you learn its tie-break rule, and your signals stop mattering.

## 8. Controls and feedback **[proposed]**

- Mouse: click a train to select it; click a siding to set the route (preview appears); *Hold* / *Depart* buttons; four signal buttons. Keyboard shortcuts 1–4 for signals.
- Feedback: animated trains and switches, the crossing indicator (free / locked / contested), a signal board with both signals, a per-beat log line ("Partner held its train for your Urgent"), the score, and a beat counter.

## 9. Presentation **[proposed]**

2D browser game. Flat, readable shapes in a *Mini Metro*-like style. Minimal sound: switch clicks and arrival chimes from a CC0 pack. No narrative beyond a one-screen tutorial.

## 10. Scope

- **Included:** one junction, one shared crossing, four signals, the model partner, the scripted partner, 3 tuning schedules and 2 held-out schedules, route preview, end screen, traces.
- **Excluded:** human-vs-human networking, procedural maps, free-text or voice chat, a campaign, learning across games, mobile layout, music.
- **Cut first, in order:** (1) smooth animation (trains jump between cells); (2) end-screen statistics beyond the score; (3) the second waiting position (one per side); (4) six trains instead of eight.

## 11. Open questions

1. **[open]** Is the shared crossing enough to make yielding interesting, or does a fixed alternation always win? *Answer by:* paper or greybox test and solver comparison in week 1.
2. **[open]** Are 24 beats and 8 trains the right length? *Answer by:* the first playtest.
3. **[open]** Does the model partner beat the scripted one on held-out schedules, or only feel different? *Answer by:* the automated comparison plus within-player playtests.
4. **[open]** Should the agent see the partner's last few commitments after they resolve? Currently yes, as public history. *Answer by:* the Design Lead review before G2.
5. **[open]** Tech stack (see `CONFIGURATION.md` §4). *Answer by:* my decision before the first build task.

## 12. References and attribution

| Reference | Specific moment or mechanic | What I borrow / change | Terms |
|---|---|---|---|
| *Hanabi* (Antoine Bauza, 2010) | Hints are the only way to share hidden information | Borrow: communication restricted to a few typed messages. Change: signals are free each round, not rationed tokens; no cards. | Game rules are not copyrightable; no name, art or text used. |
| *Keep Talking and Nobody Explodes* (Steel Crate Games, 2015) | One player sees the bomb, the other the manual | Borrow: split information forces communication. Change: an AI partner; both sides have a job. | Commercial; nothing reused. |
| *Mini Metro* (Dinosaur Polo Club, 2015) | Stations and lines as flat, colour-coded shapes | Borrow: visual readability principles. Change: my own shapes and layout. | Commercial; no assets used. |
| *Overcooked* (Ghost Town Games, 2016) | Co-op under time pressure | Borrow: interdependent co-op. Change: turn-based beats instead of real time (Pillar 3). | Commercial; nothing reused. |
| Hanabi Challenge (Bard et al., *Artificial Intelligence*, 2020) | Theory of mind under imperfect information | Motivates the question, not a claim that the agent will reason like a person. | Research paper; cited. |
| Overcooked-AI (Carroll et al., NeurIPS 2019) | Agents that coordinate with themselves can struggle with humans | Why I compare with a competent scripted partner and test with real players. | Paper cited; its MIT-licensed code is not used. |
| Single-line railway working and interlocking | One train on a shared section; interlocks prevent conflicts | Borrow: the interlock rule. | Real-world practice; no source text used. |
| My Zork midterm (`midterms/rohitr4547`) | Bounded player interface, runner with step/call/timeout limits, evaluation with held-out cases | Reuse the patterns (and possibly the model adapter). | My own work, MIT. |
| Kenney sound and UI packs (planned) | Clicks and chimes | Use as-is. | CC0; credited anyway. |
