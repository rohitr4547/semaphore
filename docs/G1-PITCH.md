# Semaphore: G1 Concept pitch (deck content and recording script)

Target length: about 4 min 20 s at a relaxed pace (≈ 620 words). Each slide lists what is on screen, then what I say. Edit the spoken lines into your own words before recording.

---

## Slide 1: Title (≈ 10 s)

**On screen:** *Semaphore*, a two-dispatcher rail junction. "G1 Concept · rohitr4547 · COMS E6998 · November 2026". A simple drawing of two sides of a junction sharing one crossing.

**Say:** "This is Semaphore, my final-project concept, and I'm asking for approval to build a prototype."

## Slide 2: The X (≈ 20 s)

**On screen:** "Semaphore is a small animated co-op game in which you and an AI partner run opposite sides of a rail junction, sharing one stretch of track and talking only through four signals, and you feel the satisfaction of getting a difficult handover right."

**Say:** read the X, then: "The whole game is about one shared crossing and the moment you decide whether to go or let your partner go first."

## Slide 3: Player and motivation (≈ 25 s)

**On screen:** "For co-op puzzle players (*Overcooked*, *Keep Talking*, *Hanabi*). Motivation: Fellowship, then Challenge and mastery. **If it works:** a player holds their own train so the partner's urgent train can cross, unprompted, and can say why afterwards."

**Say:** "The motivation is fellowship, through interdependence: the goal is hard to reach alone. My observable hypothesis is that a playtester holds their own train, without being told to, so the partner's urgent train can cross, and can explain why afterwards."

## Slide 4: Core loop (≈ 25 s)

**On screen:** the loop as a cycle: **Signal** (both send one of four signals) → **Reveal** → **Commit** (choose a train, route with preview, hold or depart; hidden from the partner) → **Advance** (one beat; trains move; interlock; score) → read the result → next signal.

**Say:** "Each round, we both send a signal and see each other's. Then we each commit a route and choose hold or depart, without seeing the other's choice. The engine advances one beat and animates it. I read whether my partner went or waited, and plan the next handover. Beats wait for both of us, so it's calm, not frantic."

## Slide 5: Pillars and concepts considered (≈ 30 s)

**On screen:** Pillars: (1) Every handover is a decision. (2) You can read your partner. (3) Calm, not frantic. Considered: *The Other Adventurer* (text race against a rival with a notebook), *Pit Wall* (AI crew chief for a slot-car racer), *Market Whispers* (AI tipsters with hidden motives).

**Say:** "Three pillars guide my choices. I considered three other concepts. The text race was text-only, and a rival that studies you is the most common agent on the board. Pit Wall needed decisions faster than a 1.5-second model call. Market Whispers was too close to a classmate's project. Semaphore won because a cooperative partner is less explored, turn-based beats tolerate model latency, and it's easy to compare with a scripted dispatcher."

## Slide 6: References and attribution (≈ 25 s)

**On screen:** the GDD §12 table, shortened: *Hanabi* (restricted hints under hidden information; rules only, no art or name), *Keep Talking* (split information; nothing reused), *Mini Metro* (readable flat lines and stations; style principles only, my own shapes), *Overcooked* (interdependent co-op; turn-based instead of real time), Hanabi Challenge and Overcooked-AI papers, railway interlocking, my own Zork midterm code (MIT), Kenney CC0 sounds.

**Say:** "From Hanabi I borrow communication limited to a few typed messages, but signals aren't rationed. From Mini Metro, readability principles, with my own shapes. From Overcooked, interdependence, but turn-based. The Overcooked-AI research found agents that coordinate with themselves struggle with people, which is why I test with real players. No commercial assets are reused; sounds are CC0."

## Slide 7: Systems diagram (≈ 30 s)

**On screen:** `docs/systems-diagram.svg`.

**Say:** "The engine owns the world state, including both queues and the future schedule. I see my side. The agent gets one filtered view of its own side: its trains and deadlines, the crossing, both signals and recent history. The dashed frame is its knowledge boundary: it never sees my queue, the schedule or the seed. Every action from either of us goes through validation, and an invalid or timed-out agent call becomes a labelled safety hold, never a made-up move."

## Slide 8: Agent hypothesis (≈ 35 s)

**On screen:** "**Behaviour:** the partner chooses its signal and whether to hold or depart, from its own side's view. **Experience:** players feel the partner cooperates, reconsidering who should go first when priorities change. **Baseline:** a competent scripted dispatcher: earliest deadline first, yields to Urgent, fixed tie-break. Same view, same actions. **Test:** each playtester plays one shift with each partner, order alternated. *When you both wanted the crossing, could you tell why your partner went or waited, and did it change what you signalled next?* **Lost with the baseline:** after a few shifts you learn its tie-break, and your signals stop mattering."

**Say:** read it, then: "If the scripted dispatcher does just as well, that's a real result, and I'd ship whichever partner players prefer."

## Slide 9: Scope (≈ 20 s)

**On screen:** Included: one junction, one crossing, four signals, model and scripted partners, 3 tuning + 2 held-out schedules, route preview, end screen, traces. Excluded: networking, procedural maps, free-text or voice, campaign, cross-game learning, mobile, music. Cut first: smooth animation → end-screen stats → one waiting position → six trains.

**Say:** "Small on purpose: one junction and one partner. If time runs short, I cut animation polish first, never the comparison."

## Slide 10: Budgets (≈ 20 s)

**On screen:** 48 calls per shift; ≈ 1,500 input + 60 output tokens per call; **≈ $0.03 per shift, ≈ $0.09 per three-shift session** at $0.30 / $2.50 per million tokens (Vertex, read Nov 4); **≈ 1.5 s per call** (measured in my midterm), 6 s timeout; added waiting ≈ 0 s median because calls run while the player decides. Evaluation before G2 ≤ $2.

**Say:** summarise the numbers and say where they come from.

## Slide 11: Risk Profile (≈ 25 s)

**On screen:** IP/Concept Medium · Business/Audience Medium · Technology Medium · Team Medium · **Genre/Mechanics High** · **Timeline High**. "Both Highs share one cause: the core loop is unproven." Mitigation: paper prototype and a solver check (does fixed alternation always win?) before building the client; greybox before polish; cut list.

**Say:** "My two High risks share a cause: if choosing who uses the crossing isn't interesting, everything else is wasted. So the first week tests exactly that, on paper and with a solver, before I build the client."

## Slide 12: Decision requested (≈ 20 s)

**On screen:** "**Approve:** build the greybox prototype: engine, scripted and model partners, browser client, automated comparison on held-out schedules, and one external playtest by G2. **A Hold would need:** evidence that the crossing creates real choices (a paper test or solver result), or a smaller scope." Small print: "Decisions marked as mine vs my agents' recommendations: `docs/PROJECT-PLAN.md`."

**Say:** "I'm asking for approval to build the greybox prototype and run the comparison and one external playtest by G2. If it's a Hold, I'd expect to come back with the paper-test and solver evidence, or a smaller scope. Thank you."

---

**Recording checklist:** 3–5 minutes; screen shows the deck; say "Gate 1", "pillars", "scripted baseline", "Risk Profile" (course vocabulary); no names or data of playtesters; upload unlisted and set to Columbia viewers.
